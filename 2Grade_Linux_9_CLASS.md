# 👨‍💻리눅스와 유닉스

## 10장 리눅스 고급 프로그래밍

### 시스템 프로그래밍
- 운영체제 기능(커널)을 직접 사용하는 프로그래밍
- 
| 분야 | 핵심 |
|------|------|
| 시스템 프로그래밍 | 커널 기능 사용 |
| 프로세스 관리 | 프로그램 실행 관리 |
| IPC | 프로세스 통신 |
| 멀티스레드 | 동시 작업 |
| 소켓 | 네트워크 통신 |
| 메모리 관리 | 동적 메모리 |
| 파일 시스템 | 파일/디렉토리 처리 |
| 데몬 | 백그라운드 서비스 |

### 시스템 프로그래밍 - 파일 처리

| 함수 | 기능 |
|------|------|
| open | 파일 열기 |
| read | 파일 읽기 |
| write | 파일 쓰기 | 
| close | 파일 닫기 |

```
#include <stdio.h>
#include <unistd.h>

int main()
{
  int fd;

  fd = open("test.txt", O_WRONLY | O_CREAT, 0644);
  write(fd, "Hello Linux\n", 13);
  close(fd);
  return 0;
}

O_WRONLY : Write Only 쓰기 전용
O_CREAT : 파일 없으면 생성
0644 : 0이 붙은 이유는 8진수 이기 때문
 - 파일 권한 rw-r--r-- 644
cmd에서 644, 코드에서 0644
```

### 시스템 프로그래밍 - 프로세스 생성
- 프로세스 = 실행 중인 프로그램
- 운영체제는 여러 프로세스를 동시에 관리
- 
| 함수 | 기능 |
|------|------|
| fork | 프로세스 생성 |
| getpid | PID 확인 |
| wait | 자식 종료 대기 |
  - fork : 현재 프로세스 복사해서 새 프로세스 하나 더 생성
```
#include <stdio.h>
#include <unistd.h>

int main()
{
  fork();

  printf("Hello\n");
  return 0;
}
```
- 프로세스가 2개가 되었기 때문에 각각 출력
- fork() = 현재 실행 중인 프로세스를 복제(copy)
  - 원래 프로세스 1개
  - fork() 실행 후
  - 똑같은 프로세스가 하나 더 생김
- 부모 프로세스 : 원래 프로세스             
- 자식 프로세스 : 새로 복제된 프로세스
```
fork();
printf("Hello\n");

부모 : (1) Hello
자식 : (2) Hello
```
```
#include <stdio.h>
#include <unistd.h>

int main()
{
  int pid;

  pid = fork();

  if(pid == 0)
  {
    printf("자식 프로세스\n");
  }
  else
  {
    printf("부모 프로세스\n");
  }

  return 0;
}
```
- 
| 반환값 | 의미 |
|--------|------|
| 0 | 자식 프로세스 |
| 양수 | 부모 프로세스 |
| -1 | 실패 |

### 시스템 프로그래밍 - IPC 통신
- IPC = Inter Proccess Communication
- 
| IPC | 설명 |
|------|-----|
| pipe | 파이프 |
| shared memory | 공유 메모리 |
| message queue | 메시지 큐 |
```
#include <stdio.h>
#include <unistd.h>

int main()
{
  int fd[2]; // 파이프 입구/출구 2개 필요
  char buf[20];

  pipe(fd); // 운영체제가 파이프 생성

  write(fd[1], "hello", 6); // 파이프에 hello 저장

  read(fd[0], buf, sizeof(buf)); // 파이프에서 데이터 읽어서 buf에 저장

  printf("%s\n", buf); // buf ← hello

  return 0;
}
```
- 
| 번호 | 의미 |
|------|------|
| fd[0] | 읽기(데이터 읽는 곳) |
| fd[1] | 쓰기(데이터 넣는 곳) |

### 시스템 프로그래밍 - 멀티 스레드
- 하나의 프로그램 안에서 여러 작업 동시 수행
- 
| 함수 | 기능 |
|------|------|
| pthread_create | 스레드 생성 |
| pthread_join | 종료 대기 |
```
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// 스레드가 실행할 함수
void* thread_function(void* arg)
{
  char* name = (char*)arg;

  for (int i = 1; i <= 5; i++)
  {
    printf("%s : %d\n", name, i);
    sleep(1); // 1초 대기
  }

  return NULL;
}
```
```
int main()
{
  pthread_t t1, t2;

  // 스레드 생성
  pthread_create(&t1, NULL, thread_function, "Thread A");
  pthread_create(&t2, NULL, thread_function, "Thread B");

  // 스레드 종료 대기
  pthread_join(t1, NULL);
  pthread_join(t2, NULL);

  printf("모든 스레드 종료\n");

  return 0;
}
```

### 시스템 프로그래밍 - 소켓 프로그래밍
- 네트워크 통신 프로그램 작성
```
socket → bind → listen → accpet
```
```
#include <stdio.h>
#include <unistd.h> 
#include <arpa/inet.h>

int main()
{
  int server_fd;

  struct sockaddr_in addr;

  server_fd = socket(AF_INET, SOCK_STREAM, 0);

  addr.sin_family = AF_INET;
  addr.sin_port = htons(9000);
  addr.sin_addr.s_addr = INADDR_ANY;

  bind(server_fd, (struct sockaddr*)&addr, sizeof(addr));

  listen(server_fd, 5);

  printf("server ready\n");

  return 0;
}
```
- 컴퓨터끼리 네트워크로 연결하는 통신 도구
  - #include <unistd.h> : close 같은 시스템 호출                          
  - #include <arpa/inet.h> : 소켓 관련 함수 사용                        
  - int server_fd : 서버 소켓 번호 저장
  - struct sockaddr_in addr : 서버 IP/포트 정보 저장
  - server_fd = socket(AF_INET, SOCK_STREAM, 0) : 소켓 생성    
  - addr.sin_port = htons(9000) : 9000번 포트 번호 사용                    
  - htons() Host TO Network Short : 바이트 순서 변환           
  - addr.sin_addr.s_addr = INADDR_ANY : 어떤 IP든 접속 허용               
  - bind(server_fd, (struct sockaddr*)&addr, sizeof(addr)) : 소켓에 IP + 포트 연결
  - listen(server_fd, 5) : 클라이언트 접속 대기
  - 5 : 동시 대기 가능 개수
- 
| 옵션 | 의미 |
|------|------|
| AF_INET | IPv4 인터넷 |
| SOCK_STREAM | TCP 방식 |
| 0 | 기본 프로토콜 |

```
int client_fd;
client_fd = accept(server_fd, NULL, NULL);

클라이언트 접속 허용
```
- 1. socket 생성
  2. 포트 연결(bind)
  3. 접속 대기(listen)
  4. 연결 허용(accept)
  5. 데이터 송수신(read / write)

### 시스템 프로그래밍 - 메모리 관리
- 프로그램 실행 중 메모리를 동적으로 사용
```
#include <stdio.h>
#include <stdlib.h>

int main()
{
  int* p;

  p = (int*)malloc(sizeof(int));

  *p = 100;

  printf("%d\n", *p);

  free(p);

  return 0;
}
```

### 시스템 프로그래밍 - 파일 시스템
- 리눅스는 모든 것을 파일처럼 관리
```
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>

int main()
{
  int ret;

  ret = mkdir("linux", 0755);

  if(ret == 0)
  {
    printf("디렉토리 생성 성공\n");
  }
  else
  {
    printf("디렉토리 생성 실패\n");
  }

  return 0;
}
```
- 
| 함수 | 기능 |
|------|------|
| mkdir | 디렉토리 생성 |
| stat | 파일 정보 |
| unlink | 파일 삭제 |

### 시스템 프로그래밍 - 데몬 서버 프로그램
- 백그라운드에서 계속 실행되는 프로그램
```
#include <stdio.h>

int main()
{
  daemon(0, 0);

  while(1)
  {
    sleep(5);
  }

  return 0;
}
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-09
