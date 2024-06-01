# Wykład 2: Komunikacja TCP typu Klient-Serwer (Java/Python)

- [[#Krótki opis komunikacji]]
	- [[#Pytanie odnośnie socketów]]
- [[#Klient TCP (Java)|Klient TCP (Java)]]
	- [[#Przykład implementacji klienta]]
- [[#Serwer TCP (Java)|Serwer TCP (Java)]]
	- [[#Przykład implementacji serwera]]
- [[#Klient TCP (Python)|Klient TCP (Python)]]
- [[#Serwer TCP (Python)|Serwer TCP (Python)]]

## Krótki opis komunikacji

Abstrakcyjne kroki tworzenia klienta i serwera TCP:
**Klient:**

1. **Utworzenie gniazda:** Klient tworzy gniazdo, które będzie punktem końcowym komunikacji.
    
2. **Nawiązanie połączenia:** Klient inicjuje połączenie z serwerem, podając jego adres i port.
    
3. **Wymiana danych:** Po nawiązaniu połączenia, klient i serwer mogą wymieniać dane w obie strony.
    
4. **Zamknięcie połączenia:** Po zakończeniu komunikacji, klient zamyka połączenie.
    

**Serwer:**

1. **Utworzenie gniazda:** Serwer tworzy gniazdo, które będzie nasłuchiwać na określonym porcie.
    
2. **Bindowanie:** Serwer wiąże gniazdo z konkretnym adresem IP i portem.
    
3. **Nasłuchiwanie:** Serwer zaczyna nasłuchiwać na określonym porcie, oczekując na połączenia od klientów.
    
4. **Akceptowanie połączeń:** Gdy klient próbuje się połączyć, serwer akceptuje połączenie, tworząc nowe gniazdo dedykowane dla tego klienta.
    
5. **Wymiana danych:** Serwer może teraz wymieniać dane z klientem przez to nowe gniazdo.
    
6. **Zamknięcie połączenia:** Po zakończeniu komunikacji, serwer zamyka połączenie z klientem.

### Pytanie odnośnie socketów
> [!question]- Pytanie
> Nie rozumiem tylko jednej rzeczy, skoro klient łączy się do socketa serwera który nasłuchuje, to na który socket klient potem wysyła wiadomości?

> [!tip]- Odpowiedź (Gemini)
> Masz rację, wyjaśnienie może być nieco mylące. W rzeczywistości, gdy klient łączy się z serwerem, serwer tworzy _nowe_ gniazdo specjalnie dla tego klienta. To nowe gniazdo jest połączone z gniazdem klienta, tworząc dedykowany kanał komunikacji między nimi.
>
> Oto jak to działa krok po kroku:
>
> 1. **Serwer nasłuchuje:** Serwer tworzy gniazdo i "wiąże" je z konkretnym adresem IP i portem. Następnie zaczyna nasłuchiwać na tym porcie, czekając na przychodzące połączenia. To gniazdo nazywamy "gniazdem nasłuchującym".
> 2. **Klient łączy się:** Klient tworzy gniazdo i nawiązuje połączenie z serwerem, podając jego adres IP i port. To połączenie jest nawiązywane z gniazdem nasłuchującym serwera.
> 3. **Serwer akceptuje połączenie:** Gdy serwer wykryje przychodzące połączenie, akceptuje je. W tym momencie tworzy _nowe_ gniazdo, które jest połączone z gniazdem klienta. To nowe gniazdo nazywamy "gniazdem połączenia".
> 4. **Komunikacja:** Teraz klient i serwer mają po jednym gnieździe (klient ma swoje oryginalne gniazdo, a serwer ma nowe gniazdo połączenia). Te dwa gniazda są połączone i tworzą dwukierunkowy kanał komunikacji. Klient wysyła dane na swoje gniazdo, a te dane są przekazywane przez gniazdo połączenia do serwera. Analogicznie, serwer wysyła dane na swoje gniazdo połączenia, a te dane są przekazywane do gniazda klienta.
>
> **Podsumowanie:**
> 
> - Gniazdo nasłuchujące serwera służy tylko do akceptowania nowych połączeń.
> - Po zaakceptowaniu połączenia, serwer tworzy nowe gniazdo połączenia dla komunikacji z klientem.
> - Klient i serwer komunikują się przez swoje gniazda, które są połączone ze sobą.

## Klient TCP (Java)

1. **Tworzenie gniazda:**
    - Użyj klasy `Socket`, przekazując adres IP serwera (np., "127.0.0.1" dla localhost) i numer portu (np., 3301).
    - Przykład: `Socket clientSocket = new Socket("127.0.0.1", 3301);`
2. **Nawiązanie połączenia:**
    - Połączenie jest nawiązywane automatycznie podczas tworzenia obiektu `Socket`.
3. **Wysyłanie i odbieranie danych:**
    - Uzyskaj strumienie wejścia/wyjścia z obiektu `Socket`:
        - `OutputStream outputStream = clientSocket.getOutputStream();`
        - `InputStream inputStream = clientSocket.getInputStream();`
    - Użyj klas takich jak `PrintWriter` (do wysyłania tekstu) lub `DataOutputStream` (do wysyłania danych binarnych) do opakowania strumienia wyjściowego.
    - Użyj klas takich jak `BufferedReader` (do odczytu tekstu) lub `DataInputStream` (do odczytu danych binarnych) do opakowania strumienia wejściowego.
    - Przykład (wysyłanie tekstu):
        ```Java
        PrintWriter out = new PrintWriter(outputStream, true);
        out.println("Hello, server!"); 
        ```
        
    - Przykład (odbieranie tekstu):
        ```Java
        BufferedReader in = new BufferedReader(new InputStreamReader(inputStream));
        String serverResponse = in.readLine();
        ```
        
4. **Zamykanie gniazda:**
    - Użyj metody `close()` obiektu `Socket` po zakończeniu komunikacji.
    - Przykład: `clientSocket.close();`

### Przykład implementacji klienta
```Java
import java.io.*;
import java.net.*;

public class TCPClient {
    public static void main(String[] args) {
	    // Połączenie z serwerem (localhost, port 3301)
        try (Socket clientSocket = new Socket("127.0.0.1", 3301); 
             PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()))) {

            String serverResponse;
            // Odczyt odpowiedzi od serwera
            while ((serverResponse = in.readLine()) != null) { 
                System.out.println(serverResponse);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Serwer TCP (Java)

1. **Tworzenie gniazda serwera:**
    - Użyj klasy `ServerSocket`, przekazując numer portu, na którym serwer będzie nasłuchiwał (np., 3301).
    - Przykład: `ServerSocket serverSocket = new ServerSocket(3301);`
2. **Nasłuchiwanie połączeń:**
    - Użyj metody `accept()` obiektu `ServerSocket`, aby czekać na przychodzące połączenie od klienta. Metoda ta blokuje się, dopóki klient się nie połączy.
    - Przykład: `Socket clientSocket = serverSocket.accept();`
3. **Akceptowanie połączeń:**
    - Metoda `accept()` zwraca nowy obiekt `Socket`, który reprezentuje połączenie z klientem.
4. **Wysyłanie i odbieranie danych:** (Tak samo jak w przypadku klienta, ale używając gniazda zwróconego przez `accept()`)
5. **Zamykanie gniazd:**
    - Użyj metody `close()` obiektów `Socket` (zarówno klienta, jak i serwera) po zakończeniu komunikacji.
    - Przykład:
        ```Java
        clientSocket.close(); 
        serverSocket.close();
        ```

### Przykład implementacji serwera
```Java
import java.io.*;
import java.net.*;

public class TCPServer {
    public static void main(String[] args) {
	    // Nasłuchiwanie na porcie 3301
        try (ServerSocket serverSocket = new ServerSocket(3301)) { 
            System.out.println("Serwer oczekuje na połączenie...");

            while (true) {
	            // Akceptacja połączenia
                try (Socket clientSocket = serverSocket.accept(); 
                     PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
                     BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()))) {

                    System.out.println("Klient połączony.");
                    // Wysłanie powitania do klienta
                    out.println("Witaj kliencie!"); 
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```


## Klient TCP (Python)

1. **Import biblioteki:**    
    ```py
    import socket
    ```
2. **Tworzenie gniazda:**
    ```py
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    ```
    - `socket.AF_INET` wskazuje, że używamy IPv4.
    - `socket.SOCK_STREAM` wskazuje, że używamy protokołu TCP.
3. **Nawiązanie połączenia:**
    ```py
    server_address = ('localhost', 3301)  # Adres serwera (localhost) i port (3301)
    client_socket.connect(server_address)
    ```
4. **Wysyłanie i odbieranie danych:**
    ```py
    message = "Hello, server!"
    client_socket.sendall(message.encode())  # Wysyłanie danych (zakodowanych jako bajty)
    
    data = client_socket.recv(1024)  # Odbieranie danych (maksymalnie 1024 bajty)
    print('Received:', data.decode())  # Dekodowanie i wyświetlanie danych
    ```
5. **Zamykanie gniazda:**
    ```py
    client_socket.close()
    ```
## Serwer TCP (Python)

1. **Import biblioteki:** (Tak samo jak w przypadku klienta)
    
2. **Tworzenie gniazda:** (Tak samo jak w przypadku klienta)
    
3. **Bindowanie gniazda:**
    ```py
    server_address = ('localhost', 3301)  # Adres serwera (localhost) i port (3301)
    server_socket.bind(server_address)
    ```
4. **Nasłuchiwanie połączeń:**
    ```py
    server_socket.listen(1)  # Maksymalnie jedno oczekujące połączenie
    ```
5. **Akceptowanie połączeń:**
    ```py
    print('Waiting for a connection...')
    connection, client_address = server_socket.accept()  # Blokuje się, aż do nawiązania połączenia
    ```
6. **Wysyłanie i odbieranie danych:**
    ```py
    while True:
        data = connection.recv(1024)  # Odbieranie danych od klienta
        if not data: break  # Zakończenie pętli, jeśli klient zamknął połączenie
        print('Received:', data.decode())
    
        message = "Hello, client!"
        connection.sendall(message.encode())  # Wysyłanie danych do klienta
    ```
7. **Zamykanie gniazd:**
    ```py
    connection.close()
    server_socket.close()
    ```

#ps #it