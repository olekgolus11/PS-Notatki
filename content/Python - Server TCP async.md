```py
import asyncio  
  
server_address = ('localhost', 3000)  
  
  
async def share_messages(reader, writer):  
    # 5. Share messages
    while True:  
        data = await reader.read(100)  
        if not data:  
            break  
        print(f'Received from {writer.get_extra_info("peername")}: ', data.decode())  
        writer.write(data)  
        await writer.drain()  
    writer.close()  
  
  
async def run_server():  
    # 1. Init socket  
    # 2. Bind socket    # 3. Listen for connections    # (All under the hood of start_server)    
    server = await asyncio.start_server(share_messages, *server_address)  
  
    # 4. Accept clients  
    async with server:  
        await server.serve_forever()  
  
  
asyncio.run(run_server())
```