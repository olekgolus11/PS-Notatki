# Server TCP with threads vs Server TCP async

## Różnice:
1. Init socket, Bind socket oraz Listen to connections
	- W asynchronicznej wersji jest pod abstrakcją `asyncio.start_server()`
4. Accept clients
	- Wersja asynchroniczna uruchamia funkcję `server.serve_forever()` która w pętli acceptuje klientów i przy każdym accept uruchamia funkcję (podaną w argumencie `asyncio.start_server()`) w tle.
5. Share messages
	- Wykorzystywane są inne funkcje, w wersji asynchronicznej korzystamy z abstrakcji na socketowe funkcje

## Server TCP with threads
![[Python - Server TCP with threads]]
## Server TCP async
![[Python - Server TCP async]]

#it #ps