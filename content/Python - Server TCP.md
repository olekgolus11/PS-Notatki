```py
import socket  
  
server_address = ('localhost', 3000)  
  
# 1. Init socket  
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
  
# 2. Bind socket  
server_socket.bind(server_address)  
  
# 3. Listen to connections  
server_socket.listen(1)  
  
# 4. Accept client  
connection, client_address = server_socket.accept()  
  
# 5. Share messages  
while True:  
    data = connection.recv(1024)  
    if not data:  
        break  
    print('Received: ', data.decode())  
    connection.send(data)  
  
connection.close()  
server_socket.close()
```
