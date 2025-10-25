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
# import a couple of libraries in the same time 

request_queue_size = 5 # max 5 users
# size of queue, i chose 5 


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


# two paramenters come into function client connection and port 
client_connection is a socket which communicate with client 
recv(1024) is number of binary which come through the socket 
decode() because we have to trasfer binary into plain languege 

The next step is ports which we can choose to get access to different content
port: 8888, 8000, 8881

client_connection.sendall(http_response)
# this method sends all the data until eveything has gone through 

