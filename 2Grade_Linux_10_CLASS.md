# 👨‍💻리눅스와 유닉스

## 11장 리눅스 소켓 프로그래밍

### 클라이언트-서버 모델
- 네트워크 응용 프로그램
  - 클라이언트-서버 모델을 기반으로 동작
- 클라이언트-서버 모델
  - 하나의 서버 프로세스와 여러 개의 클라이언트로 구성
  - 서버는 어떤 자원을 관리하고 클라이언트를 위해 자원 관련 서비스를 제공
```
              1. 클라이언트가 요청을 보냄
  (클라이언트---------------------------→(서버
    프로세스)←---------------------------프로세스)←----→(자원)
4. 클라이언트가     3. 서버가 응답을 보냄           2. 서버가
   응답을 처리함                                      요청을 처리함
```

### 소켓의 종류
- 소켓
  - 네트워크 통신을 위한 통로(Endpoint)
  - 소켓을 이용하여 데이터를 송수신함
  - 네트워크에 대한 사용자 수준의 인터페이스를 제공
  - 소켓은 양방향 통신 방법으로 클라이언트-서버 모델을 기반으로 프로세스 사이의 통신에 매우 적합
- 유닉스 소켓(AF_UNIX)
  - 같은 호스트 내의 프로세스 사이의 통신 방법
- 인터넷 소켓(AF_INET)
  - 인터넷에 연결된 서로 다른 호스트에 있는 프로세스 사이의 통신 방법

### TCP 소켓 프로그래밍
- TCP는 연결(Connection)을 맺은 후 통신함
- 특징
  - 연결 지향(Connection-Oriented)
  - 신뢰성 보장
  - 데이터 순서 보장
  - 데이터 손실 시 재전송
- 사용 예
  - SSH
  - 웹(HTTP/HTTPS)
  - FTP
  - 데이터베이스 접속

### UDP 소켓 프로그래밍
- UDP는 연결 없이 데이터를 보냄
- 특징
  - 비연결형(Connectionless)
  - 빠름
  - 신뢰성 보장 안 함
  - 순서 보장 안 함
- 사용 예
  - DNS
  - 온라인 게임
  - 실시간 스트리밍
  - VoIP
```
int sockfd;
sockfd = socket(AF_INET, SOCK_DGRAM, 0);    →  UDP소켓 생성
sockfd = socket(AF_INET, SOCK_STREAM, 0);   →  TCP소켓 생성
```

### 호스트(Host)란?
- 네트워크에서 호스트는 네트워크에 연결된 장치를 말함
```
PC 1      → 호스트
PC 2      → 호스트
웹 서버    → 호스트
DB 서버    → 호스트
```
  - 같은 호스트 내 통신의 의미
```
예를 들어 한 리눅스 서버에서 2개의 서버가 동시에 실행되고 있다고 가정
  - 웹 서버 (Apache)
  - DB 서버 (MySQL)

호스트 A (192.168.0.100)
  - Apache (80)
  - MySQL  (3306)
Apache가 MySQL에 접속할 때 (Apache → MySQL) 통신은 일어나지만 같은 서버 내부에서 이루어짐.
이를 같은 호스트 내 통신이라고 함

- 같은 호스트 연결 : 한 컴퓨터 내부에서 프로세스까지 통신(localhost, 127.0.0.1)
- 서로 다른 호스트 연결 : 네트워크를 통해 다른 컴퓨터와 통신(192.168.x.x, 공인 IP 등)
```

### 소켓 연결
- 1. 서버가 소켓을 만듦
  2. 클라이언트가 소켓을 만든 후 서버에 연결 요청을 함
  3. 서버가 클라이언트의 연결 요청을 수락하여 소켓 연결이 이루어짐
```
(서버)-----ㅑ

(서버)-----ㅑ ∃-----(클라이언트)

(서버)-----ㅑ
      -----ㅁ-----(클라이언트)
```

### 소켓 연결 과정
- 서버
  - 1. socket() 호출을 이용하여 소켓을 만들고 이름을 붙임
    2. listen() 호출을 이용하여 대기큐를 만듦
    3. 클라이언트로부터 연결 요청을 accept() 호출을 이용하여 수락
    4. 소켓 연결이 이루어지면
       - 자식 프로세스를 생성하여
       - 클라이언트로부터 요청 처리
       - 클라이언트에게 응답
- 클라이언트
  - 1. socket() 호출을 이용하여 소켓을 만듦
    2. connection() 호출을 이용하여 서버에 연결 요청을 함
    3. 서버가 연결 요청을 수락하면 소켓 연결이 만듦
    4. 서버에 서비스를 요청하고 서버로부터 응답을 받아 처리

```
 클라이언트               서버
  [socket]             [socket]
     |                    ↓  
     |                  [bind]
     |                    ↓
     |                 [listen]
     |       연결 요청     ↓          
 [connect]------------→[accept]←------|
     ↓                    ↓           |
[서비스 요청]------→[서비스 요청 처리]  | 
     ↓                    ↓           |
 [응답 처리]←---------[서비스 응답]     | 다음 클라이언트로부터
     ↓                    |           |  연결 요청을 기다림
  [close]-----------------↓           |
                       [close]--------|
```

### 소켓 만들기
```
int socket(int domain, int type, int protocol)
소켓을 생성하고 소켓을 위한 파일 디스크립터를 리턴, 실패하면 -1을 리턴
```
- 인터넷 소켓
  - fd = socket(AF_INET, SOCK_STREAM, DEFAULT_PROTOCOL);
- 유닉스 소켓
  - fd = socket(AF_UNIX, SOCK_STREAM, DEFAULT_PROTOCOL);

- 인터넛에서 연결이 성립하려면
  - 서로 다른 호스트가 통신하려면 IP주소, 포트번호가 필요함
  - IP 주소 : 상대방 주소를 알아야 함
  - 포트번호 : 어떤 서비스인지 알아야 함
  - 예시
    - 22  →  SSH
    - 80  →  HTTP
    - 443 →  HTTPS

### 소켓에 이름(주소) 적기
```
int bind(int fd, struct sockaddr* address, int addressLen)
소켓에 대한 이름 바인딩이 성공하면 0을 실패하면 -1을 리턴
```
- 유닉스 소켓 이름
```
struct sockaddr_un {
  unsigned short sun_family; // AF_UNIX
  char sun_path[108]; // 소켓 이름
}
```
- 인터넷 소켓 이름
```
struct sockaddr_in {
  unsigned short sin_family; // AF_INET
  unsigned short sin_port; // 인터넷 소켓의 포트 번호
  struct in_addr sin_addr; // 32-bit IP 주소
}
```

### 소켓 큐 생성
- listen() 시스템 호출
  - 클라이언트로부터의 연결 요청을 기다림
  - 연결 요청 대기 큐의 길이를 정함
```
int listen(int fd, int queueLength)
소켓 fd에 대한 연결 요청을 기다린다, 성공하면 0을 실패하면 -1을 리턴
```
```
listen(sockfd, 5);

클라이언트1 → 대기
클라이언트2 → 대기
클라이언트3 → 대기
클라이언트4 → 대기
클라이언트5 → 대기
```

### 소켓에 연결 요청
- connect() 시스템 호출
  - fd가 나타내는 클라이언트 소켓과 address가 나타내는 서버 소켓과의 연결을 요청
  - 성공하면 fd를 서버 소켓과의 통신에 사용할 수 있음
```
int connect(int fd, struct sockaddr* address, int addressLen)
성공하면 0을 실패하면 -1를 리턴
```

### 연결 요청 수락
```
int accept(int fd, struct sockaddr* address, int* addressLen)
성공하면 새로 만들어진 복사본 소켓의 파일 디스크립터, 실패하면 -1을 리턴
```
- 서버가 클라이언트로부터의 연결요청을 수락하는 내부 과정
  - 1. 서버는 fd가 나타내는 서버 소켓을 경청하고
    2. 클라이언트의 연결 요청이 올 때까지 기다림
    3. 클라이언트로부터 연결 요청이 오면 원래 서버 소켓과 같은 복사본 소켓을 만들어 이 복사본 소켓과 클라이언트 소켓을 연결
    4. 연결이 이루어지면 address는 클라이언트 소켓의 주소로 세팅되고 addressLen는 그 크기로 세팅
    5. 새로 만들어진 복사본 소켓의 파일 디스크립터를 리턴
- 파일 디스크립터(File Descriptor)
  - 운영체제가 파일, 소켓, 파이프 등의 입출력 자원을 관리하기 위해 사용하는 정수 번호
  - 즉, 운영체제가 열린 파일이나 소켓에 붙여주는 번호표

### 시스템 프로그래밍 - 소켓 프로그래밍
- 네트워크 통신 프로그램 작성
```
socket → bind → listen → accept
```
- 소켓 프로그래밍(서버 코드)
```
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

int main()
{
  int server_fd, client_fd;
  char buffer[1024];
  struct sockaddr_in server_addr;

  // 소켓 생성
  server_fd = socket(AF_INET, SOCK_STREAM, 0);

  // 서버 주소 설정
  server_addr.sin_family = AF_INET;
  server_addr.sin_addr.s_addr = INADDR_ANY;
  server_addr.sin_port = htons(9000);

  // bind
  bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr));

  // 접속 대기
  listen(server_fd, 5);

  printf("서버 실행 중...\n");

  // 클라이언트 접속 허용
  client_fd = accept(server_fd, NULL, NULL);

  printf("클라이언트 접속 성공\n");

  // 데이터 수신
  recv(client_fd, buffer, sizeof(buffer), 0);

  printf("받은 메시지 : %s\n", buffer);

  close(client_fd);
  close(server_fd);

  return 0;
}
```
- 소켓 프로그래밍(클라이언트 코드)
```
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

int main()
{
  int sockfd;

  struct sockaddr_in server_addr;

  sockfd = socket(AF_INET, SOCK_STREAM, 0);

  server_addr.sin_family = AF_INET;
  server_addr.sin_port = htons(9000);

  inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr);

  connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr));

  char msg[] = "Hello Server";

  send(sockfd, msg, strlen(msg)+1, 0);

  close(sockfd);

  reutn 0;
}
```
- 서버 컴파일
  - gcc server.c -o server
- 클라이언트 컴파일
  - gcc client.c -o client
- 실행
  - 터미널1 에서 ./server
  - 터미널2 에서 ./client
 
### 파이썬 서버 코드(server.py)
```
import socket

s = socket.socket()
s.bind(("0.0.0.0", 9000))
s.listen(1)
print("서버 대기중")
client, addr = s.accept()
data = client.recv(1024)
print(data.decode())
```

### 파이썬 클라이언트 코드(client.py)
```
import socket

s = socket.socket()
s.connect(("127.0.0.1", 9000))
s.send("안녕하세요".encode())
```
- 서버 컴파일
  - Python3 server.py
- 클라이언트 컴파일
  - Python3 client.py
- 실행 결과
  - 서버창에 "안녕하세요" 출력
 
##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-09
