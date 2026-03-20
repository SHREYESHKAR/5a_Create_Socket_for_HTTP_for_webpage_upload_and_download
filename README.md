# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
### program.py
```
import socket

def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request.encode())
        response = s.recv(4096).decode(errors='ignore')
    return response


def upload_file(host, port, filename):
    with open(filename,'rb') as file:
        file_data = file.read()
        content_length = len(file_data)

        request = f"POST /upload HTTP/1.1\r\nHost: {host}\r\nContent-Length: {content_length}\r\n\r\n"
        request = request.encode() + file_data

        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
            s.connect((host, port))
            s.sendall(request)
            response = s.recv(4096).decode(errors='ignore')

    return response


def download_file(host, port, filename):
    request = f"GET /{filename} HTTP/1.1\r\nHost: {host}\r\n\r\n"
    response = send_request(host, port, request)

    parts = response.split('\r\n\r\n', 1)
    if len(parts) > 1:
        file_content = parts[1]
        with open(filename, 'wb') as file:
            file.write(file_content.encode())


if __name__ == "__main__":
    host = 'example.com'
    port = 80

    # Upload file
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:\n", upload_response)

    # Download file
    download_file(host, port, 'example.txt')
    print("File downloaded successfully.")
```
### example.txt
```
Hello this is CN Lab file transfer
```
## OUTPUT
<img width="1615" height="464" alt="image" src="https://github.com/user-attachments/assets/204c3a99-700f-4332-97a1-da323502b7f2" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
