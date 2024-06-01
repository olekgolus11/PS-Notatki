# Wykład 1: Techniki programowania z wykorzystaniem gniazd

- [[#Model OSI (Open Systems Interconnection)|Model OSI (Open Systems Interconnection)]]
- [[#Model TCP/IP|Model TCP/IP]]
- [[#Protokoły TCP i UDP|Protokoły TCP i UDP]]
- [[#Gniazda (Sockets)|Gniazda (Sockets)]]
	- [[#Elementy definiujące gniazda:]]
	- [[#Tryby transmisji]]
	- [[#Relacje między komputerami]]
	- [[#Uchwyty (Handles)]]
	- [[#Bufory gniazd]]
	- [[#Funkcje gniazd]]
- [[#DNS (Domain Name System)|DNS (Domain Name System)]]


## Model OSI (Open Systems Interconnection)

Model OSI to koncepcyjny model opisujący warstwy komunikacji sieciowej. Składa się z siedmiu warstw:

1. **Warstwa fizyczna:** Zajmuje się przesyłaniem surowych bitów przez medium transmisyjne (kable, światłowody itp.).
    
2. **Warstwa łącza danych:** Odpowiada za organizację danych w ramki, wykrywanie i korekcję błędów transmisji.
    
3. **Warstwa sieciowa:** Zajmuje się routingiem pakietów danych w sieci, wyborem optymalnej ścieżki.
    
4. **Warstwa transportowa:** Zapewnia niezawodną (TCP) lub zawodną (UDP) transmisję danych między aplikacjami.
    
5. **Warstwa sesji:** Zarządza sesjami komunikacyjnymi, umożliwiając synchronizację i odtwarzanie połączeń.
    
6. **Warstwa prezentacji:** Odpowiada za formatowanie danych, szyfrowanie, kompresję.
    
7. **Warstwa aplikacji:** Interfejs dla aplikacji użytkownika, udostępniający usługi sieciowe (HTTP, FTP, SMTP itp.).
    

## Model TCP/IP

Model TCP/IP to praktyczna implementacja modelu OSI, składająca się z czterech warstw:

1. **Warstwa aplikacji:** Odpowiada warstwie aplikacji i prezentacji w modelu OSI.
    
2. **Warstwa transportowa:** Odpowiada warstwie transportowej w modelu OSI.
    
3. **Warstwa Internetu (sieciowa):** Odpowiada warstwie sieciowej w modelu OSI.
    
4. **Warstwa dostępu do sieci:** Odpowiada warstwom łącza danych i fizycznej w modelu OSI.
    

## Protokoły TCP i UDP

- **TCP (Transmission Control Protocol):** Protokół zorientowany na połączenie, zapewniający niezawodną transmisję danych. Wykorzystuje potwierdzenia odbioru, retransmisje w przypadku błędów, kontrolę przepływu.
    
- **UDP (User Datagram Protocol):** Protokół bezpołączeniowy, oferujący szybszą, ale mniej niezawodną transmisję danych. Nie gwarantuje dostarczenia pakietów ani ich kolejności.
    

## Gniazda (Sockets)

Gniazda to abstrakcyjny interfejs programistyczny (API) umożliwiający komunikację między procesami za pośrednictwem sieci. Są podstawowym mechanizmem komunikacji w protokołach TCP i UDP.

### Elementy definiujące gniazda:

- **Adres IP:** Unikalny adres identyfikujący hosta w sieci.
    
- **Numer portu:** Liczba identyfikująca aplikację lub usługę na hoście.
    
- **Para gniazdowa:** Kombinacja adresu IP i numeru portu, identyfikująca połączenie sieciowe.
    

### Tryby transmisji

- **Unicast:** Transmisja danych od jednego nadawcy do jednego odbiorcy.
    
- **Multicast:** Transmisja danych od jednego nadawcy do grupy odbiorców.
    
- **Broadcast:** Transmisja danych od jednego nadawcy do wszystkich hostów w sieci.
    

### Relacje między komputerami

- **Serwer-klient:** Model, w którym serwer udostępnia usługi wielu klientom.
    
- **Peer-to-peer (P2P):** Model, w którym każdy komputer może pełnić rolę zarówno klienta, jak i serwera.
    

### Uchwyty (Handles)

Uchwyty to wskaźniki do struktur danych opisujących zasoby systemowe (np. pliki, gniazda). Umożliwiają zarządzanie tymi zasobami przez system operacyjny.

### Bufory gniazd

Gniazda posiadają bufory do przechowywania danych podczas transmisji. Bufory te mogą się przepełnić, co powoduje blokowanie lub opóźnienia w komunikacji.

### Funkcje gniazd

Istnieje wiele funkcji służących do pracy z gniazdami, takich jak:

- `socket`: Tworzenie nowego gniazda.
    
- `bind`: Przypisanie adresu IP i portu do gniazda.
    
- `listen`: Przełączenie gniazda w tryb nasłuchiwania.
    
- `accept`: Akceptowanie połączenia przychodzącego.
    
- `connect`: Nawiązanie połączenia z innym gniazdem.
    
- `send`, `recv`: Wysyłanie i odbieranie danych.
    
- `close`: Zamykanie gniazda.
    

## DNS (Domain Name System)

DNS to system służący do tłumaczenia nazw domenowych (np. [www.example.com](https://www.example.com)) na adresy IP. Jest niezbędny do działania Internetu.

#ps #it