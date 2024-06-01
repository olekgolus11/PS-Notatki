# Przykładowy test z Programowania Sieciowego 2

## Część I: Pytania zamknięte (1 punkt za każdą poprawną odpowiedź)

1. **Która z poniższych funkcji służy do przypisania adresu IP i portu do gniazda w języku C?**
    
    a. `listen()`
    
    b. `accept()`
    
    c. `bind()`
    
    d. `connect()`

> [!NOTE]- Odpowiedź
> c. `bind()`
> 
> Funkcja `bind()` służy do przypisania adresu IP i portu do gniazda, co jest niezbędne do działania serwera.
    
2. **Wskaż poprawną kolejność wywołań funkcji podczas tworzenia serwera TCP:**
    
    a. `socket()`, `bind()`, `listen()`, `accept()`
    
    b. `bind()`, `socket()`, `listen()`, `accept()`
    
    c. `socket()`, `listen()`, `bind()`, `accept()`
    
    d. `socket()`, `connect()`, `bind()`, `listen()`

> [!NOTE]- Odpowiedź
> a. `socket()`, `bind()`, `listen()`, `accept()`
> 
> Prawidłowa kolejność to: utworzenie gniazda (`socket()`), zbindowanie go (`bind()`), nasłuchiwanie na połączenia (`listen()`) i akceptowanie połączeń (`accept()`).
    
3. **Która klasa w Javie reprezentuje gniazdo datagramowe (UDP)?**
    
    a. `Socket`
    
    b. `ServerSocket`
    
    c. `DatagramSocket`
    
    d. `DatagramPacket`

> [!NOTE]- Odpowiedź
> c. `DatagramSocket`
> 
> Klasa `DatagramSocket` w Javie reprezentuje gniazdo datagramowe, używane do komunikacji UDP.
    
4. **Co to jest adres rozgłoszeniowy (broadcast address)?**
    
    a. Adres IP używany do wysyłania danych do wszystkich urządzeń w sieci.
    
    b. Adres IP używany do wysyłania danych do wybranej grupy urządzeń.
    
    c. Adres IP używany do wysyłania danych tylko do jednego urządzenia.
    
    d. Adres IP, który nie jest przypisany do żadnego urządzenia.

> [!NOTE]- Odpowiedź
> a. Adres IP używany do wysyłania danych do wszystkich urządzeń w sieci.
> 
> Adres rozgłoszeniowy to specjalny adres IP, który umożliwia wysyłanie danych do wszystkich urządzeń w sieci lokalnej.
    
5. **Która metoda w C# służy do ustawiania opcji gniazda?**
    
    a. `GetSocketOption()`
    
    b. `SetSocketOption()`
    
    c. `GetStream()`
    
    d. `Close()`

> [!NOTE]- Odpowiedź
> b. `SetSocketOption()`
> 
> Metoda `SetSocketOption()` w C# służy do ustawiania opcji gniazda, takich jak czas oczekiwania (timeout) czy rozmiar bufora.
    
6. **W programowaniu asynchronicznym, co oznacza słowo kluczowe `await` w C#?**
    
    a. Wstrzymuje wykonywanie metody asynchronicznej do momentu zakończenia oczekiwanej operacji.
    
    b. Uruchamia nową operację asynchroniczną.
    
    c. Zgłasza wyjątek, jeśli operacja asynchroniczna zakończy się niepowodzeniem.
    
    d. Definiuje metodę jako asynchroniczną.

> [!NOTE]- Odpowiedź
> a. Wstrzymuje wykonywanie metody asynchronicznej do momentu zakończenia oczekiwanej operacji.
> 
> Słowo kluczowe `await` w C# wstrzymuje wykonywanie metody asynchronicznej do momentu zakończenia oczekiwanej operacji asynchronicznej.
    
7. **Co to jest "livelock" w kontekście wielowątkowości?**
    
    a. Sytuacja, w której wątek jest blokowany przez inny wątek i nie może kontynuować działania.
    
    b. Sytuacja, w której dwa lub więcej wątków zmieniają swój stan, ale nie robią postępu.
    
    c. Sytuacja, w której wątek nigdy nie otrzymuje czasu procesora.
    
    d. Sytuacja, w której wątek wykonuje się zbyt długo.

> [!NOTE]- Odpowiedź
> b. Sytuacja, w której dwa lub więcej wątków zmieniają swój stan, ale nie robią postępu.
> 
> Livelock to sytuacja, w której dwa lub więcej wątków ciągle zmieniają swój stan, ale nie robią postępu, zazwyczaj próbując rozwiązać konflikt dostępu do zasobu.
    

## Część II: Pytania otwarte (5 punktów za każdą poprawną odpowiedź)

1. Opisz, jak zaimplementować prosty serwer wielowątkowy TCP w Javie, który obsługuje wielu klientów jednocześnie.

> [!NOTE]- Odpowiedź
> 1. Tworzymy obiekt `ServerSocket`, który nasłuchuje na porcie 3301.
> 2. W pętli nieskończonej akceptujemy nowe połączenia (`serverSocket.accept()`).
> 3. Dla każdego nowego połączenia tworzymy nowy wątek (za pomocą wyrażenia lambda i `new Thread(...).start()`), który wykonuje metodę `handleClient`.
> 4.  Metoda `handleClient` obsługuje komunikację z klientem: odbiera dane (`in.readLine()`) i wysyła je z powrotem (`out.println()`).
>
> ```java
> import java.io.*;
> import java.net.*;
> 
> public class ThreadedEchoServer {
>    public static void main(String[] args) {
>        try (ServerSocket serverSocket = new ServerSocket(3301)) {
>            System.out.println("Server is listening on port 3301...");
>
>            while (true) {
>                Socket clientSocket = serverSocket.accept();
>                new Thread(() -> handleClient(clientSocket)).start(); 
>            }
>        } catch (IOException e) {
>            e.printStackTrace();
>        }
>    }
>
>    private static void handleClient(Socket socket) {
>        try (socket;
>             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
>             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {
>
>            String inputLine;
>            while ((inputLine = in.readLine()) != null) {
>                out.println(inputLine);
>            }
>        } catch (IOException e) {
>            e.printStackTrace();
>        }
>    }
> }

2. Wyjaśnij, czym jest `Promise` w JavaScript i jak jest używany w programowaniu asynchronicznym. Podaj przykład użycia `Promise` do obsługi wyniku zapytania AJAX.

> [!NOTE]- Odpowiedź
> `Promise` to obiekt reprezentujący ewentualny wynik operacji asynchronicznej. Może on być w jednym z trzech stanów:
>
> - **pending:** operacja jest w toku.
> - **fulfilled:** operacja zakończyła się sukcesem, zwracając wartość.
> - **rejected:** operacja zakończyła się niepowodzeniem, zwracając błąd.
>
> `Promise` pozwala na obsługę wyniku operacji asynchronicznej za pomocą metod `.then()`, `.catch()` i `.finally()`:
>
> - `.then(onFulfilled, onRejected)`: rejestruje funkcje zwrotne (callbacks), które zostaną wywołane po zakończeniu operacji (sukces lub błąd).
>    
> - `.catch(onRejected)`: rejestruje funkcję zwrotną, która zostanie wywołana tylko w przypadku błędu.
>    
> - `.finally(onFinally)`: rejestruje funkcję zwrotną, która zostanie wywołana zawsze, niezależnie od wyniku operacji.
    
3. Omów różnice między rozgłaszaniem (broadcasting) a multicastingiem w komunikacji UDP. Podaj przykłady zastosowań obu technik.

> [!NOTE]- Odpowiedź
> ![](https://i.imgur.com/cW0bOI2.png)


#ps #it