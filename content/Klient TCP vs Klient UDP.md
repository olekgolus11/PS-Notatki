# Klient TCP vs Server UDP

## Różnice:
1. Init socket
	- `SOCK_STREAM` (flaga od TCP) a `SOCK_DGRAM` (flaga od UDP - DGRAM jako datagram)
2. Connect to server socket
	- Client UDP nie łączy się do serwera, ponieważ UDP jest bezpołączeniowe
3. Share messages
	- Client UDP wykorzystuje funkcję `socket.sendto()` ponieważ przyjmuje ona dodatkowy parametr w postaci adresu socketu serwera

## Client TCP
![[Python - Client TCP]]
## Client UDP
![[Python - Client UDP]]

#it #ps