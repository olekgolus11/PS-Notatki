```py
import socket  
  
server_address = ('localhost', 3001)  
  
# 1. Init socket  
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  
  
# (!) No connecting (!)  
  
# 2. Share messages  
while True:  
    message = input()  
    if message == 'exit':  
        break  
    client_socket.sendto(message.encode(), server_address)  
    data = client_socket.recv(1024)  
    print(data.decode())  
  
client_socket.close()
```