# Server TCP vs Server TCP with threads

## Różnice:
1. Init socket
	- Bez zmian
2. Bind socket
	- Bez zmian
3. Listen to connections
	- Z uwagi na to że możemy operować kilkoma klientami naraz to możemy nasłuchiwać na więcej połączeń (choć nie musimy)
4. Accept client
	- Bez zmian
5. Share messages
	- Zamiast wymieniać wiadomości w pętli głównego programu to puszczamy całą pętlę do nowego wątku

## Server TCP
![[Python - Server TCP]]
## Server TCP with threads
![[Python - Server TCP with threads]]

#it #ps