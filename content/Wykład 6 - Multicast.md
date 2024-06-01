# Multicast
- [[#Multicasting w komunikacji UDP (Python i Java)|Multicasting w komunikacji UDP (Python i Java)]]
	- [[#Python]]
	- [[#Java]]


## Multicasting w komunikacji UDP (Python i Java)

**Multicasting** to metoda komunikacji sieciowej, w której wiadomość jest wysyłana do grupy urządzeń, które dołączyły do określonej grupy multicastowej. W przeciwieństwie do broadcastu, który wysyła wiadomość do wszystkich urządzeń w sieci, multicast pozwala na bardziej selektywne dostarczanie komunikatów.

### Python

**Wysyłanie multicastu:**
```py
import socket

multicast_group = ('224.0.0.251', 3303)  # Adres grupy multicast i port

# Tworzenie gniazda
with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as sock:
    # Ustawienie czasu życia (TTL) pakietów multicast
    ttl = struct.pack('b', 1)  # 1 oznacza, że pakiety nie będą przesyłane poza sieć lokalną
    sock.setsockopt(socket.IPPROTO_IP, socket.IP_MULTICAST_TTL, ttl)

    # Wysyłanie komunikatu
    message = b'Hello, multicast group!'
    sent = sock.sendto(message, multicast_group)
```

1. Definiujemy adres grupy multicast (`multicast_group`) i port.
2. Tworzymy gniazdo UDP (`socket.socket`).
3. Ustawiamy czas życia (TTL) pakietów multicast za pomocą `setsockopt`.
4. Wysyłamy wiadomość (`message`) do grupy multicast za pomocą `sendto()`.

**Odbieranie multicastu:**
```py
import socket
import struct

multicast_group = '224.0.0.251'  # Adres grupy multicast
server_address = ('', 3303)  # Nasłuchujemy na wszystkich interfejsach na porcie 3303

# Tworzenie gniazda
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(server_address)

# Dołączanie do grupy multicast
group = socket.inet_aton(multicast_group)
mreq = struct.pack('4sL', group, socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

# Odbieranie komunikatów
while True:
    data, address = sock.recvfrom(1024)
    print(f'Received {len(data)} bytes from {address}: {data.decode()}')
```

1. Definiujemy adres grupy multicast i adres serwera (nasłuchujemy na wszystkich interfejsach).
2. Tworzymy gniazdo UDP.
3. Bindujemy gniazdo do adresu serwera.
4. Dołączamy do grupy multicast za pomocą `setsockopt` i struktury `ip_mreq`.
5. W pętli nieskończonej odbieramy datagramy za pomocą `recvfrom()`.
6. Wyświetlamy otrzymaną wiadomość wraz z adresem nadawcy.

### Java

**Wysyłanie multicastu:**
```java
import java.net.*;

public class MulticastSender {
    public static void main(String[] args) throws IOException {
        InetAddress group = InetAddress.getByName("224.0.0.251");
        int port = 3303;

        DatagramSocket socket = new DatagramSocket();
        String message = "Hello, multicast group!";
        byte[] buffer = message.getBytes();

        DatagramPacket packet = new DatagramPacket(buffer, buffer.length, group, port);
        socket.send(packet);
        socket.close();
    }
}
```

1. Pobieramy adres grupy multicast (`InetAddress.getByName`).
2. Tworzymy gniazdo `DatagramSocket`.
3. Przygotowujemy dane, adres grupy i port.
4. Tworzymy pakiet `DatagramPacket` i wysyłamy go.
5. Zamykamy gniazdo.

**Odbieranie multicastu:**
```java
import java.net.*;

public class MulticastReceiver {
    public static void main(String[] args) throws IOException {
        InetAddress group = InetAddress.getByName("224.0.0.251");
        int port = 3303;

        MulticastSocket socket = new MulticastSocket(port);
        socket.joinGroup(group);

        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

        socket.receive(packet);
        String received = new String(packet.getData(), 0, packet.getLength());
        System.out.println("Received multicast: " + received);
        socket.leaveGroup(group);
        socket.close();
    }
}
```

1. Pobieramy adres grupy multicast.
2. Tworzymy gniazdo `MulticastSocket`, nasłuchujące na określonym porcie.
3. Dołączamy do grupy multicast za pomocą `joinGroup()`.
4. Przygotowujemy bufor i pakiet do odbioru danych.
5. Odbieramy pakiet za pomocą `receive()`.
6. Wyświetlamy otrzymaną wiadomość.
7. Opuszczamy grupę multicast (`leaveGroup()`) i zamykamy gniazdo.