```py
import socket  
import threading  
  
server_address = ('localhost', 3000)  
threads = []  
  
def share_messages(connection, client_address):  
    while True:  
        data = connection.recv(1024)  
        if not data:  
            break  
        print(f'Received from {client_address}: ', data.decode())  
        connection.send(data)  
    connection.close()  
  
  
# 1. Init socket  
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  
  
# 2. Bind socket  
server_socket.bind(server_address)  
  
# 3. Listen to connections  
server_socket.listen(5)  
  
try:  
    while True:  
        # 4. Accept clients  
        connection, client_address = server_socket.accept()  
  
        # 5. Share messages (in a new thread)  
        handle_client_thread = threading.Thread(target=share_messages, args=(connection, client_address))  
        handle_client_thread.start()  
        threads.append(handle_client_thread)  
except Exception:  
    print("Program terminated")  
finally:  
    for thread in threads:  
        thread.join()  
    server_socket.close()
```
