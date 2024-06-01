# Broadcast
- [[#Broadcasting w komunikacji UDP (Python i Java)|Broadcasting w komunikacji UDP (Python i Java)]]
	- [[#Python]]
	- [[#Java]]


## Broadcasting w komunikacji UDP (Python i Java)

**Broadcast** to metoda komunikacji sieciowej, w której wiadomość jest wysyłana do wszystkich urządzeń w sieci lokalnej. W kontekście UDP (User Datagram Protocol), broadcast jest realizowany poprzez wysłanie datagramu UDP na specjalny adres rozgłoszeniowy (broadcast address).

### Python

**Wysyłanie broadcastu:**
```py
import socket

broadcast_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
broadcast_socket.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)  # Włączenie trybu broadcast

broadcast_message = b"This is a broadcast message"
broadcast_address = ('<broadcast_address>', 3303)  # Adres broadcast i port

broadcast_socket.sendto(broadcast_message, broadcast_address)
broadcast_socket.close()
```

1. Tworzymy gniazdo UDP (`socket.socket`).
2. Ustawiamy opcję gniazda `SO_BROADCAST` na 1, aby umożliwić wysyłanie broadcastów.
3. Definiujemy wiadomość (`broadcast_message`) i adres rozgłoszeniowy (`broadcast_address`). Adres rozgłoszeniowy zależy od konfiguracji sieci (np., `192.168.1.255`).
4. Wysyłamy wiadomość na adres rozgłoszeniowy za pomocą `sendto()`.
5. Zamykamy gniazdo.

**Odbieranie broadcastu:**
```py
import socket

receiver_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
receiver_socket.bind(('', 3303))  # Bindowanie do dowolnego adresu na porcie 3303

while True:
    data, address = receiver_socket.recvfrom(1024)
    print(f"Received broadcast from {address}: {data.decode()}")
```

1. Tworzymy gniazdo UDP.
2. Bindujemy gniazdo do portu 3303, na którym będziemy nasłuchiwać broadcastów. Pusty ciąg znaków ('') oznacza, że nasłuchujemy na wszystkich dostępnych interfejsach sieciowych.
3. W pętli nieskończonej odbieramy datagramy za pomocą `recvfrom()`.
4. Wyświetlamy otrzymaną wiadomość wraz z adresem nadawcy.

### Java

**Wysyłanie broadcastu:**
```java
import java.net.*;

public class UDPBroadcastSender {
    public static void main(String[] args) throws IOException {
        DatagramSocket socket = new DatagramSocket();
        socket.setBroadcast(true);

        byte[] buffer = "This is a broadcast message".getBytes();
        InetAddress address = InetAddress.getByName("255.255.255.255"); // Adres broadcast
        int port = 3303;

        DatagramPacket packet = new DatagramPacket(buffer, buffer.length, address, port);
        socket.send(packet);
        socket.close();
    }
}
```

1. Tworzymy gniazdo `DatagramSocket`.
2. Włączamy tryb broadcast za pomocą `setBroadcast(true)`.
3. Przygotowujemy dane, adres rozgłoszeniowy i port.
4. Tworzymy pakiet `DatagramPacket` i wysyłamy go.
5. Zamykamy gniazdo.

**Odbieranie broadcastu:**
```java
import java.net.*;

public class UDPBroadcastReceiver {
    public static void main(String[] args) throws IOException {
        DatagramSocket socket = new DatagramSocket(3303);

        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        socket.receive(packet);
        String received = new String(packet.getData(), 0, packet.getLength());
        System.out.println("Received broadcast: " + received);
        socket.close();
    }
}
```

1. Tworzymy gniazdo `DatagramSocket`, nasłuchujące na porcie 3303.
2. Przygotowujemy bufor i pakiet do odbioru danych.
3. Odbieramy pakiet za pomocą `receive()`.
4. Wyświetlamy otrzymaną wiadomość.
5. Zamykamy gniazdo.

#ps #it