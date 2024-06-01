# Przykładowe zaliczenie (wygenerowane przez AI)


## Część I: Pytania zamknięte (1 punkt za każdą poprawną odpowiedź)

1. **Która warstwa modelu OSI odpowiada za dostarczanie danych między aplikacjami, zapewniając niezawodność lub jej brak?**
    
    a. Warstwa fizyczna
    
    b. Warstwa sieciowa
    
    c. Warstwa transportowa
    
    d. Warstwa aplikacji

> [!NOTE]- Odpowiedź
> c. Warstwa transportowa
> 
> Warstwa transportowa odpowiada za dostarczanie danych między aplikacjami, oferując protokoły TCP (niezawodny) i UDP (zawodny).

2. **Który protokół jest zorientowany na połączenie i zapewnia niezawodność transmisji?**
    
    a. UDP
    
    b. IP
    
    c. TCP
    
    d. HTTP

> [!NOTE]- Odpowiedź
> c. TCP
> 
> TCP (Transmission Control Protocol) jest protokołem zorientowanym na połączenie, zapewniającym niezawodność transmisji dzięki potwierdzeniom, retransmisjom i kontroli przepływu.

3. **Co to jest "para gniazdowa"?**
    
    a. Dwa gniazda połączone ze sobą.
    
    b. Adres IP i numer portu.
    
    c. Adres IP i adres MAC.
    
    d. Nazwa hosta i numer portu.

> [!NOTE]- Odpowiedź
> b. Adres IP i numer portu
> 
> Para gniazdowa to kombinacja adresu IP i numeru portu, identyfikująca unikalny punkt końcowy komunikacji w sieci.
    
4. **Jaka jest różnica między gniazdem nasłuchującym a gniazdem połączenia w kontekście serwera TCP?**
    
    a. Nie ma różnicy, to tylko nazwy.
    
    b. Gniazdo nasłuchujące czeka na połączenia, a gniazdo połączenia służy do komunikacji z klientem.
    
    c. Gniazdo połączenia czeka na połączenia, a gniazdo nasłuchujące służy do komunikacji z klientem.
    
    d. Gniazdo nasłuchujące jest używane tylko przez UDP, a gniazdo połączenia tylko przez TCP.

> [!NOTE]- Odpowiedź
> b. Gniazdo nasłuchujące czeka na połączenia, a gniazdo połączenia służy do komunikacji z klientem.
> 
> Gniazdo nasłuchujące czeka na przychodzące połączenia od klientów, a po nawiązaniu połączenia serwer tworzy nowe gniazdo połączenia dedykowane dla komunikacji z tym konkretnym klientem.
    
5. **Która metoda w Javie służy do akceptowania połączenia przychodzącego na serwerze TCP?**
    
    a. `socket()`
    
    b. `bind()`
    
    c. `listen()`
    
    d. `accept()`

> [!NOTE]- Odpowiedź
> d. `accept()`
> 
> Metoda `accept()` w Javie służy do akceptowania połączenia przychodzącego na serwerze TCP.
    
6. **W programowaniu asynchronicznym, co to jest "Future" (Java) lub "Task" (C#)?**
    
    a. Mechanizm blokowania wątku.
    
    b. Reprezentacja wyniku operacji asynchronicznej.
    
    c. Sposób na uniknięcie wyjątków.
    
    d. Metoda synchronizacji wątków.

> [!NOTE]- Odpowiedź
> b. Reprezentacja wyniku operacji asynchronicznej.
> 
> Zarówno `Future` w Javie, jak i `Task` w C# reprezentują wynik operacji asynchronicznej, który może być dostępny w przyszłości.
    
7. **Która technika służy do wysyłania komunikatów do wszystkich urządzeń w sieci lokalnej?**
    
    a. Unicast
    
    b. Multicast
    
    c. Broadcast
    
    d. Anycast

> [!NOTE]- Odpowiedź
> c. Broadcast
> 
> Broadcast to technika rozsyłania komunikatów do wszystkich urządzeń w sieci lokalnej.
    
8. **Jaka jest główna zaleta wielowątkowości w programowaniu sieciowym?**
    
    a. Zmniejszenie zużycia pamięci.
    
    b. Obsługa wielu klientów jednocześnie.
    
    c. Uniknięcie konieczności używania socketów.
    
    d. Szybsze działanie pojedynczego klienta.

> [!NOTE]- Odpowiedź
> b. Obsługa wielu klientów jednocześnie.
> 
> Wielowątkowość pozwala serwerowi na obsługę wielu klientów jednocześnie, zwiększając wydajność i responsywność aplikacji.
    
9. **Który mechanizm synchronizacji wątków pozwala na ograniczenie liczby wątków wykonujących jednocześnie dany fragment kodu?**
    
    a. Sekcja krytyczna
    
    b. Mutex
    
    c. Semafor
    
    d. Monitor

> [!NOTE]- Odpowiedź
> a. Sekcja krytyczna
    
10. **Który problem wielowątkowości występuje, gdy dwa lub więcej wątków wzajemnie blokują się, czekając na zasoby, których posiadają inne wątki?**
    
    a. Livelock
    
    b. Deadlock
    
    c. Race condition
    
    d. Starvation

> [!NOTE]- Odpowiedź
> d. Starvation

## Część II: Pytania otwarte (5 punktów za każdą poprawną odpowiedź)

1. Opisz różnice między protokołami TCP i UDP oraz podaj przykłady zastosowań każdego z nich.

> [!NOTE]- Odpowiedź
> TCP (Transmission Control Protocol) i UDP (User Datagram Protocol) to dwa podstawowe protokoły transportowe używane w komunikacji sieciowej. Różnią się one pod względem niezawodności, sposobu nawiązywania połączeń, kontroli przepływu i kolejności danych.
>
> **TCP** jest protokołem zorientowanym na połączenie, co oznacza, że przed rozpoczęciem transmisji danych nawiązywane jest połączenie między nadawcą a odbiorcą. TCP gwarantuje dostarczenie wszystkich danych w poprawnej kolejności i bez błędów. Jeśli pakiet danych zostanie zgubiony, TCP automatycznie zażąda jego retransmisji. Dodatkowo, TCP kontroluje przepływ danych, dostosowując tempo nadawania do możliwości odbiorcy, aby uniknąć przeciążenia sieci. Ze względu na te cechy, TCP jest idealny do zastosowań, gdzie niezawodność i kolejność danych są kluczowe, takich jak przeglądanie stron internetowych (HTTP), przesyłanie plików (FTP) czy poczta elektroniczna (SMTP).
> 
> **UDP** jest protokołem bezpołączeniowym, co oznacza, że dane są wysyłane bez uprzedniego nawiązania połączenia. UDP nie gwarantuje dostarczenia danych, ich kolejności ani ochrony przed błędami. Jest to protokół "ognia i zapomnienia" - nadawca wysyła datagram i nie wie, czy dotarł on do celu. UDP jest znacznie szybszy niż TCP, ponieważ nie ma narzutu związanego z nawiązywaniem i utrzymywaniem połączenia oraz kontrolą przepływu. Dzięki temu UDP jest odpowiedni do zastosowań, gdzie szybkość jest ważniejsza niż niezawodność, takich jak streaming audio/wideo, gry online czy wideokonferencje.
    
2. Wyjaśnij, jak działa bindowanie socketu w kontekście serwera sieciowego. Jakie informacje są potrzebne do zbindowania socketu?

> [!NOTE]- Odpowiedź
> Bindowanie socketu to proces **przypisania gniazda** (reprezentowanego przez obiekt `Socket`) do **konkretnego adresu IP i numeru portu** na lokalnym komputerze.
> 
> Informacje potrzebne do zbindowania socketu to:
>
> - **Adres IP:** Może to być konkretny adres IP interfejsu sieciowego (np., 192.168.1.100) lub adres specjalny `INADDR_ANY`, który oznacza nasłuchiwanie na wszystkich dostępnych interfejsach.
>    
> - **Numer portu:** To 16-bitowa liczba całkowita, która identyfikuje usługę lub aplikację nasłuchującą na danym adresie IP.
    
3. Co to jest programowanie asynchroniczne i jakie są jego zalety w programowaniu sieciowym? Podaj przykład implementacji asynchronicznego klienta sieciowego w wybranym przez siebie języku programowania.

> [!NOTE]- Odpowiedź
> Programowanie asynchroniczne to paradygmat programowania, w którym operacje mogą być wykonywane **współbieżnie**, bez blokowania głównego wątku aplikacji. Oznacza to, że aplikacja może kontynuować wykonywanie innych zadań, podczas gdy operacja asynchroniczna jest przetwarzana w tle.
>
> W kontekście programowania sieciowego, asynchroniczność jest szczególnie przydatna, ponieważ **operacje wejścia/wyjścia (I/O)**, takie jak wysyłanie i odbieranie danych przez sieć, są często **czasochłonne**. Gdybyśmy wykonywali te operacje synchronicznie, aplikacja musiałaby czekać na zakończenie każdej operacji sieciowej, co mogłoby prowadzić do "zawieszenia" interfejsu użytkownika i słabej responsywności.

#ps #it