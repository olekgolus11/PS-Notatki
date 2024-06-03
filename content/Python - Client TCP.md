```py
import socket  
  
server_address = ('localhost', 3000)  
  
# 1. Init socket  
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
  
# 2. Connect to server socket  
client_socket.connect(server_address)  
  
# 3. Share messages  
while True:  
    message = input()  
    if message == 'exit':  
        break  
    client_socket.send(message.encode())  
    print(client_socket.recv(1024).decode())  
  
client_socket.close()
```