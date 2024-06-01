# Techniki asynchroniczne
- [[#Programowanie wywołań asynchronicznych w kontekście programowania sieciowego|Programowanie wywołań asynchronicznych w kontekście programowania sieciowego]]
	- [[#Programowanie wywołań asynchronicznych w kontekście programowania sieciowego#Asynchroniczność w programowaniu sieciowym|Asynchroniczność w programowaniu sieciowym]]
	- [[#Programowanie wywołań asynchronicznych w kontekście programowania sieciowego#Implementacja asynchroniczności|Implementacja asynchroniczności]]
	- [[#Programowanie wywołań asynchronicznych w kontekście programowania sieciowego#Implementacja serwera TCP (C#)|Implementacja serwera TCP (C#)]]


## Programowanie wywołań asynchronicznych w kontekście programowania sieciowego

### Asynchroniczność w programowaniu sieciowym

- **Problem:** Operacje sieciowe (np. pobieranie danych z serwera) są czasochłonne i mogą blokować działanie aplikacji.
    
- **Rozwiązanie:** Wywołania asynchroniczne pozwalają na wykonywanie operacji sieciowych w tle, bez blokowania głównego wątku aplikacji.
    
- **Korzyści:**
    
    - Aplikacja pozostaje responsywna podczas operacji sieciowych.
        
    - Lepsze wykorzystanie zasobów (np. procesora).
        
    - Możliwość obsługi wielu żądań jednocześnie.
### Implementacja asynchroniczności

- **C#:**
    
    - Słowa kluczowe `async` i `await` ułatwiają tworzenie metod asynchronicznych.
        
    - Metoda oznaczona jako `async` może używać `await` do oczekiwania na zakończenie operacji asynchronicznej.
        
    - Typy zwracane metod asynchronicznych to zazwyczaj `Task` lub `Task<TResult>`.
        
    - Przykład:
        ```cs
        async Task<string> DownloadStringAsync(string uri)
        {
            using (HttpClient client = new HttpClient())
            {
                return await client.GetStringAsync(uri);
            }
        }
        ```
        
- **Java:**
    
    - Interfejs `Future` reprezentuje wynik operacji asynchronicznej.
        
    - Metoda `get()` pozwala na pobranie wyniku, ale może blokować wątek.
        
    - Klasa `CompletableFuture` rozszerza `Future` i oferuje więcej możliwości, takich jak łączenie zadań, obsługa błędów itp.
        
    - Przykład:
        ```java
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            // Pobierz dane z serwera
            return result;
        });
        
        // Wykonaj inne operacje
        
        String result = future.get(); // Poczekaj na wynik (może blokować)
        ```
        
- **JavaScript:**
    
    - Obiekty `Promise` reprezentują wynik operacji asynchronicznej.
        
    - Metody `then`, `catch` i `finally` pozwalają na obsługę wyniku, błędów i zakończenia operacji.
        
    - Przykład:
        ```js
        fetch('https://api.example.com/data')
            .then(response => response.json())
            .then(data => console.log(data))
            .catch(error => console.error(error));
        ```
        

### Implementacja serwera TCP (C#)
```cs
async Task ListenForClientsAsync(TcpListener listener)
{
    while (true)
    {
        TcpClient client = await listener.AcceptTcpClientAsync();
        _ = HandleClientAsync(client); // _ = oznacza, że nie czekamy na zakończenie tego zadania
    }
}

async Task HandleClientAsync(TcpClient client)
{
    using (NetworkStream stream = client.GetStream())
    using (StreamReader reader = new StreamReader(stream))
    using (StreamWriter writer = new StreamWriter(stream))
    {
        while (true)
        {
            string request = await reader.ReadLineAsync();
            if (request == null)
                break;
            string response = ProcessRequest(request);
            await writer.WriteLineAsync(response);
        }
    }
}
```

- Metoda `ListenForClientsAsync` nasłuchuje na przychodzące połączenia.
    
- Dla każdego połączenia tworzy zadanie `HandleClientAsync`, które obsługuje komunikację z klientem.
    
- `HandleClientAsync` czyta żądania od klienta (`reader.ReadLineAsync`), przetwarza je (`ProcessRequest`) i wysyła odpowiedzi (`writer.WriteLineAsync`).
    

**Uwaga:** To tylko podstawowe przykłady. W rzeczywistych aplikacjach implementacja może być bardziej złożona, uwzględniając np. buforowanie, obsługę błędów, bezpieczeństwo itp.

#ps #it