```py
import socket  
  
server_address = ('localhost', 3001)  
  
# 1. Init socket  
server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  
  
# 2. Bind socket  
server_socket.bind(server_address)  
  
# (!) No listening, no accepting (!)  
  
# 3. Share messages  
while True:  
    data, client_address = server_socket.recvfrom(1024)  
    if not data:  
        break  
    print('Received: ', data.decode())  
    server_socket.sendto(data, client_address)  
  
server_socket.close()
```