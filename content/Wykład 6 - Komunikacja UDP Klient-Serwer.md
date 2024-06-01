# Komunikacja UDP Klient-Serwer
- [[#Wykład 6: Komunikacja z użyciem protokołu UDP (Java)|Wykład 6: Komunikacja z użyciem protokołu UDP (Java)]]
	- [[#Klient UDP (Java)]]
	- [[#Serwer UDP (Java)]]
- [[#Przykład implementacji klienta i serwera UDP w Javie|Przykład implementacji klienta i serwera UDP w Javie]]
	- [[#Klient UDP (Java)]]
	- [[#Serwer UDP (Java)]]
- [[#Wykład 6: Komunikacja z użyciem protokołu UDP (Python)|Wykład 6: Komunikacja z użyciem protokołu UDP (Python)]]
	- [[#Klient UDP (Python)]]
	- [[#Serwer UDP (Python)]]
- [[#Implementacja klienta i serwera UDP w Pythonie|Implementacja klienta i serwera UDP w Pythonie]]
	- [[#Klient UDP]]
	- [[#Serwer UDP]]


## Wykład 6: Komunikacja z użyciem protokołu UDP (Java)

### Klient UDP (Java)

1. **Tworzenie gniazda (Socket):**
    
    - Użyj klasy `DatagramSocket` do utworzenia gniazda datagramowego.
    - Przykład: `DatagramSocket clientSocket = new DatagramSocket();`
2. **Przygotowanie danych:**
    
    - Utwórz tablicę bajtów (`byte[]`) zawierającą dane do wysłania.
    - Utwórz obiekt `DatagramPacket` przechowujący dane, adres IP i port docelowy.
    - Przykład:
        ```java
        byte[] sendData = "Hello, server!".getBytes();
        InetAddress address = InetAddress.getByName("127.0.0.1"); // Adres localhost
        int port = 3302;
        DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, address, port);
        ```
        
3. **Wysyłanie danych:**
    
    - Użyj metody `send()` gniazda, aby wysłać pakiet danych.
    - Przykład: `clientSocket.send(sendPacket);`
4. **Odbieranie danych (opcjonalne):**
    
    - Utwórz tablicę bajtów do przechowywania odebranych danych.
    - Utwórz obiekt `DatagramPacket` do przechowywania odebranych danych, adresu IP i portu nadawcy.
    - Użyj metody `receive()` gniazda, aby odebrać pakiet danych.
    - Przykład:
        ```java
        byte[] receiveData = new byte[1024];
        DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
        clientSocket.receive(receivePacket);
        String modifiedSentence = new String(receivePacket.getData());
        ```
        
5. **Zamykanie gniazda:**
    
    - Użyj metody `close()` gniazda po zakończeniu komunikacji.
    - Przykład: `clientSocket.close();`

### Serwer UDP (Java)

1. **Tworzenie gniazda (Socket):**
    
    - Użyj klasy `DatagramSocket`, przekazując numer portu, na którym serwer będzie nasłuchiwał.
    - Przykład: `DatagramSocket serverSocket = new DatagramSocket(3302);`
2. **Odbieranie danych:**
    
    - Utwórz tablicę bajtów do przechowywania odebranych danych.
    - Utwórz obiekt `DatagramPacket` do przechowywania odebranych danych, adresu IP i portu nadawcy.
    - Użyj metody `receive()` gniazda, aby odebrać pakiet danych.
    - Przykład:
        ```java
        byte[] receiveData = new byte[1024];
        DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
        serverSocket.receive(receivePacket);
        ```
        
3. **Przetwarzanie danych:**
    
    - Wyodrębnij dane z pakietu (`receivePacket.getData()`).
    - Przetwórz dane zgodnie z logiką aplikacji.
4. **Wysyłanie danych (opcjonalne):**
    
    - Utwórz tablicę bajtów zawierającą dane do wysłania.
    - Utwórz obiekt `DatagramPacket` przechowujący dane, adres IP i port klienta (można je uzyskać z `receivePacket.getAddress()` i `receivePacket.getPort()`).
    - Użyj metody `send()` gniazda, aby wysłać pakiet danych.
5. **Zamykanie gniazda:**
    
    - Użyj metody `close()` gniazda po zakończeniu nasłuchiwania.
    - Przykład: `serverSocket.close();`

## Przykład implementacji klienta i serwera UDP w Javie

### Klient UDP (Java)
```java
import java.io.*;
import java.net.*;

public class UDPClient {
    public static void main(String[] args) throws IOException {
        DatagramSocket clientSocket = new DatagramSocket();
        byte[] sendData = "Hello, server!".getBytes();
        InetAddress address = InetAddress.getByName("localhost"); // Adres serwera
        int port = 3302; // Port serwera

        DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, address, port);
        clientSocket.send(sendPacket);

        byte[] receiveData = new byte[1024];
        DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
        clientSocket.receive(receivePacket);

        String modifiedSentence = new String(receivePacket.getData());
        System.out.println("FROM SERVER: " + modifiedSentence);
        clientSocket.close();
    }
}
```

1. **Tworzymy gniazdo datagramowe (`DatagramSocket`)**.
2. **Przygotowujemy dane do wysłania:**
    - Konwertujemy wiadomość na tablicę bajtów.
    - Tworzymy obiekt `DatagramPacket`, który przechowuje dane, adres IP serwera (`localhost`) i port.
3. **Wysyłamy pakiet danych (`clientSocket.send(sendPacket)`).**
4. **Oczekujemy na odpowiedź od serwera:**
    - Tworzymy bufor (`receiveData`) do przechowywania odpowiedzi.
    - Tworzymy obiekt `DatagramPacket` do przechowywania odpowiedzi.
    - Odbieramy pakiet danych (`clientSocket.receive(receivePacket)`).
5. **Konwertujemy odpowiedź na tekst i ją wyświetlamy.**
6. **Zamykamy gniazdo.**

### Serwer UDP (Java)
```java

import java.io.*;
import java.net.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class UDPServer {
    public static void main(String[] args) throws IOException {
        DatagramSocket serverSocket = new DatagramSocket(3302); // Nasłuchiwanie na porcie 3302
        byte[] receiveData = new byte[1024];
        byte[] sendData;

        while (true) {
            DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
            serverSocket.receive(receivePacket);

            String sentence = new String(receivePacket.getData());
            InetAddress IPAddress = receivePacket.getAddress();
            int port = receivePacket.getPort();

            LocalDateTime currentDateTime = LocalDateTime.now();
            DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd/MM/yyyy HH:mm:ss");
            String formattedDateTime = currentDateTime.format(formatter);

            String capitalizedSentence = formattedDateTime + " - " + sentence.toUpperCase();
            sendData = capitalizedSentence.getBytes();

            DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, IPAddress, port);
            serverSocket.send(sendPacket);
        }
    }
}
```

1. **Tworzymy gniazdo datagramowe (`DatagramSocket`)**, nasłuchujące na porcie 3302.
2. **W pętli nieskończonej:**
    - Oczekujemy na przychodzące pakiety danych (`serverSocket.receive(receivePacket)`).
    - Pobieramy adres IP i port klienta.
    - Pobieramy aktualny czas i formatujemy go.
    - Konwertujemy otrzymaną wiadomość na wielkie litery i dodajemy znacznik czasu.
    - Wysyłamy zmodyfikowaną wiadomość z powrotem do klienta (`serverSocket.send(sendPacket)`).

## Wykład 6: Komunikacja z użyciem protokołu UDP (Python)

### Klient UDP (Python)

1. **Import biblioteki:**
    ```py
    import socket
    ```
    
2. **Tworzenie gniazda (Socket):**
    ```py
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) 
    ```
    
    - `socket.AF_INET` wskazuje, że używamy IPv4.
    - `socket.SOCK_DGRAM` wskazuje, że używamy protokołu UDP.
3. **Przygotowanie danych:**
    ```py
    message = b"Hello, server!"  # Dane do wysłania (zakodowane jako bajty)
    server_address = ('localhost', 3302)  # Adres i port serwera
    ```
    
4. **Wysyłanie danych:**
    ```py
    client_socket.sendto(message, server_address)
    ```
    
5. **Odbieranie danych (opcjonalne):**
    ```py
    data, server = client_socket.recvfrom(1024)  # Odbieranie danych (maks. 1024 bajty)
    print(f"Received: {data.decode()}")  # Dekodowanie i wyświetlanie danych
    ```
    
6. **Zamykanie gniazda:**
    ```py
    client_socket.close()
    ```
    

### Serwer UDP (Python)

1. **Import biblioteki:** (Tak samo jak w przypadku klienta)
    
2. **Tworzenie gniazda:** (Tak samo jak w przypadku klienta)
    
3. **Bindowanie gniazda (opcjonalne):**
    ```py
    server_address = ('localhost', 3302)  # Adres i port serwera
    server_socket.bind(server_address)  # Przypisanie adresu i portu do gniazda
    ```
    
    - Ten krok jest opcjonalny, ponieważ serwer UDP może odbierać dane bez jawnego bindowania.
4. **Odbieranie danych:**
    ```py
    data, client_address = server_socket.recvfrom(1024)  # Odbieranie danych i adresu klienta
    ```
    
5. **Przetwarzanie danych:**
    ```py
    message = data.decode().upper()  # Przetwarzanie danych (np. zmiana na wielkie litery)
    ```
    
6. **Wysyłanie danych (opcjonalne):**
    ```py
    server_socket.sendto(message.encode(), client_address)  # Wysyłanie danych do klienta
    ```
    
7. **Zamykanie gniazda:**
    ```py
    server_socket.close()
    ```

## Implementacja klienta i serwera UDP w Pythonie

### Klient UDP
```py
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

message = b"Hello, server!"
server_address = ('localhost', 3302)

client_socket.sendto(message, server_address)

data, server = client_socket.recvfrom(1024)
print(f"Received: {data.decode()}")

client_socket.close()
```

1. Importujemy moduł `socket`.
2. Tworzymy gniazdo (`socket.socket`) typu datagram (UDP) dla IPv4.
3. Definiujemy wiadomość (`message`) do wysłania (zakodowaną jako bajty) oraz adres i port serwera (`server_address`).
4. Wysyłamy wiadomość do serwera za pomocą `sendto()`.
5. Odbieramy odpowiedź od serwera za pomocą `recvfrom()`, która zwraca zarówno dane, jak i adres serwera.
6. Dekodujemy i wyświetlamy otrzymane dane.
7. Zamykamy gniazdo.

### Serwer UDP
```py
import socket
from datetime import datetime

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ('localhost', 3302)
server_socket.bind(server_address)

while True:
    data, client_address = server_socket.recvfrom(1024)
    message = data.decode().upper()
    current_time = datetime.now().strftime("%d/%m/%Y %H:%M:%S")
    response = f"{current_time} - {message}"
    server_socket.sendto(response.encode(), client_address)
```

1. Importujemy moduł `socket` oraz `datetime` do obsługi czasu.
2. Tworzymy gniazdo (`socket.socket`) typu datagram (UDP) dla IPv4.
3. Definiujemy adres i port serwera (`server_address`) i "wiążemy" gniazdo z tym adresem (`bind()`), aby serwer nasłuchiwał na określonym porcie.
4. W pętli nieskończonej:
    - Odbieramy dane i adres klienta za pomocą `recvfrom()`.
    - Dekodujemy wiadomość, konwertujemy ją na wielkie litery i dodajemy aktualny czas.
    - Wysyłamy zmodyfikowaną wiadomość z powrotem do klienta za pomocą `sendto()`.

#it #ps