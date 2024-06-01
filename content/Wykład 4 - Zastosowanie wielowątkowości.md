# Zastosowanie wielowątkowości
- [[#Implementacja wielowątkowego serwera TCP|Implementacja wielowątkowego serwera TCP]]
	- [[#Implementacja wielowątkowego serwera TCP#Python|Python]]
	- [[#Implementacja wielowątkowego serwera TCP#Java|Java]]


## Implementacja wielowątkowego serwera TCP

### Python
```py
import socket
import threading

HOST = '127.0.0.1'  # Standard loopback interface address (localhost)
PORT = 3301        # Port to listen on (non-privileged ports are > 1023)

def handle_client(conn, addr):
    with conn:  # Ensure proper cleanup using a context manager
        print(f"Connected by {addr}")
        while True:
            data = conn.recv(1024)
            if not data:
                break
            conn.sendall(data)  # Echo back the received data

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen()

    while True:
        conn, addr = s.accept()  # Accept new connections
        thread = threading.Thread(target=handle_client, args=(conn, addr))  # Create a new thread for each client
        thread.start()
```

1. Importujemy moduły `socket` i `threading`.
2. Definiujemy funkcję `handle_client`, która będzie wykonywana w osobnym wątku dla każdego klienta. Funkcja ta odbiera dane od klienta i odsyła je z powrotem.
3. Tworzymy gniazdo (`socket.socket`), bindujemy je do adresu i portu, a następnie nasłuchujemy (`listen`).
4. W pętli nieskończonej akceptujemy nowe połączenia (`s.accept()`).
5. Dla każdego nowego połączenia tworzymy nowy wątek (`threading.Thread`), przekazując mu funkcję `handle_client` i argumenty (gniazdo klienta i jego adres).
6. Uruchamiamy wątek (`thread.start()`), który będzie obsługiwał klienta niezależnie od innych wątków.

### Java
```java
import java.io.*;
import java.net.*;

public class ThreadedEchoServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(3301)) {
            System.out.println("Server is listening on port 3301...");

            while (true) {
                Socket clientSocket = serverSocket.accept();
                new Thread(() -> handleClient(clientSocket)).start();  // Start a new thread for each client
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void handleClient(Socket socket) {
        try (socket;
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {

            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                out.println(inputLine);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

1. Importujemy niezbędne klasy.
2. Tworzymy obiekt `ServerSocket`, który nasłuchuje na porcie `3301`.
3. Wewnątrz pętli `while(true)` akceptujemy nowe połączenia (`serverSocket.accept()`).
4. Dla każdego nowego połączenia tworzymy nowy wątek (za pomocą wyrażenia lambda i `new Thread(...).start()`), który wykonuje metodę `handleClient`.
5. Metoda `handleClient` obsługuje komunikację z klientem: odbiera dane (`in.readLine()`) i wysyła je z powrotem (`out.println()`).
6. Obsługujemy wyjątki `IOException`.