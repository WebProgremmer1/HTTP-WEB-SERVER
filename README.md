# HTTP-WEB-SERVER
The initial goal was to play around HTTP server and understand how it is working. So far, I learned how web server and socket operates. How web server works with server and client. 
Difference between TCP and UDP connection. 
Operation of HOST and PORT. 
Overall, experience of internet connection. 
Connect web server to different PORTS 

Before creating the program, I had requirements that i program should satisfy:
Firstly, process a couple of users and in case flow of users put them in the queue. Use different ports to connect to content, for example I chose different langueges. English, French and Russian.  
So user can use different content in 8888, 8881 and 8000 Ports. 


Description of my code:


import socket, threading 
**import a couple of libraries in the same time**

request_queue_size = 5 # max 5 users
**size of queue, i chose 5**


def handle_request(client_connection, port):
    request = client_connection.recv(1024)
    print(request.decode())
    if port == 8888:
        message = 'English. How can i help?'
    if port == 8881:
        message = 'French. Bounjour'
    if port == 8000:
        message = 'Spanish. Ca ma tase?'
    http_response = f"HTTP/1.1 200 OK\r\n\r\n{message}".encode()
    client_connection.sendall(http_response)
    client_connection.close()


**two paramenters come into function client connection and port**
client_connection is a socket which communicate with client 
recv(1024) is number of binary which come through the socket 
decode() because we have to trasfer binary into plain languege 

The next step is ports which we can choose to get access to different content
port: 8888, 8000, 8881

client_connection.sendall(http_response)
**this method sends all the data until eveything has sent**
client_connection.close()
**closing client_connection**


def serve_forever(port):
    server_address = ('', port)
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(server_address)
    listen_socket.listen(request_queue_size)
    print('Serving HTTP on port {port} ...'.format(port=PORT))
    
server_address = ('', port)
**server address is server_connection and port**
listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
**SOCK_STREAM is TCP connection and AF_INET is Internet IPv4**
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
**socket.SO_REUSEADDR means set scoket option** 
**SO_REUSEADDR allows to reuse socket and SOL_SOCKET change a general option at the socket level**
listen_socket.bind(server_address)
**attaches your socket to a specific address to IP address and a port number**
listen_socket.listen(request_queue_size)
**accepting incoming connections and in case add them to the queue list**


while True:
        client_connection, _ = listen_socket.accept()
        threading.Thread(
            target=handle_request,
            args=(client_connection, port),
            daemon=True
        ).start()

client_connection, _ = listen_socket.accept()
**client_connection is socket communicate with a client, server - client**
**client_address provides ip address and port but I have a blank because in local or test machines it is not nessary to provide these data**
threading.Thread(
            target=handle_request,
            args=(client_connection, port),
            daemon=True
        ).start()

**use some code from handle_request, args allows to accept multiple client_connections and ports**
daemon = True 
**The thread will automatically close when the main program stops**
.start()
**lauches the thread**
Without threads, each connection would block the loop — your server could only talk to one client at a time.

if __name__ == '__main__':
if the file is main and not imported, runs code below

https://ruslanspivak.com/lsbaws-part2/
https://ruslanspivak.com/lsbaws-part3/
