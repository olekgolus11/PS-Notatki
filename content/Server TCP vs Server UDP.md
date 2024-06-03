# Server TCP vs Server UDP

## Różnice:
1. Init socket
	- `SOCK_STREAM` (flaga od TCP) a `SOCK_DGRAM` (flaga od UDP - DGRAM jako datagram)
2. Bind socket
	- bez zmian
3. Listen to connections
	- Serwer UDP nie nasłuchuje bo nie nawiązuje połączeń
4. Accept client
	- Serwer UDP nie akceptuje połączeń bo ich nie nawiązuje
5. Share messages
	- Serwer UDP wykorzystuje funkcję `socket.recvfrom()` ponieważ zwraca ona dodatkowo adres socketu klienta, który wykorzystujemy później
	- Serwer UDP wykorzystuje funkcję `socket.sendto()` ponieważ przyjmuje ona dodatkowy parametr w postaci adresu socketu klienta

## Server TCP
![[Python - Server TCP]]
## Server UDP
![[Python - Server UDP]]

#it #ps