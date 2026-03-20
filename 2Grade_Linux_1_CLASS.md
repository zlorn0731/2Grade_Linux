# 👨‍💻리눅스와 유닉스

## 2장 기본 명령어

### 간단한 명령어
  - data : 현재 시각 나옴
  - hostname : 버츄얼 플랫폼 이름
  - uname : 현재 설치된 OS - Linux | uname -a : 더 상세한 운영체제 설명 나옴
  - who : 어떤 아이디로 로그인 했는지 나옴
  - clear : 지금까지 쳤던 명령어 다 삭
  - passwd : 비밀번호 변경
 
### 디렉토리 계층구조
  - 리눅스 디렉토리
```
| 디렉토리 | 설명 |
|---------|------|
| /       | 루트 디렉토리 (최상위) |
| /bin    | 기본 명령어 |
| /sbin   | 시스템 관리 명령어 |
| /etc    | 환경 설정 파일 |
| /usr    | 사용자 프로그램, 라이브러리 |
| /boot   | 부팅 관련 파일 (커널, 부트로더) |
| /dev    | 디바이스 파일 |
| /home   | 사용자 홈 디렉터리 | 🔥
| /root   | 관리자(root) 홈 디렉토리 |
| /lib    | 라이브러리 |
| /mnt    | 외부 장치 마운트 |
| /media  | 이동식 장치 마운트 (USB 등) |
| /lost+found | 손상된 파일 복구 |
| /proc   | 시스템 정보 (가상 파일 시스템) |
| /tmp    | 임시 파일 |
| /var    | 로그, 메일 등 가변 데이터 |
```

### 홈 디렉토리(home directory)
  - 각 사용자마다 별도의 홈 디렉토리가 있음
  - 사용자가 로그인하면 홈 디렉토리에서 작업을 시작함
 
### 경로명
  - ~ : 홈 디렉토리 → ~/test
  - . : 현재 디렉토리 → ./file.txt
  - .. : 부모 디렉토리 → ../test/cs1.txt
### 절대 경로명(absolute path name) : 루트 디렉토리로부터 시작하여 경로 이름을 정확하게 적는 것
### 상대 경로명(relative path name) : 현재 작업 디렉토리부터 시작해서 경로 이름을 적는 것
#### (예시)
```
/ 
├── usr
│   ├── bin
│   ├── sbin
│   └── lib
├── opt
├── dev
├── etc
├── home
│   ├── choi
│   │   └── cs101
│   └── chang
│       ├── doc
│       └── test
│           └── cs1.txt
├── kernel
└── var

cs1.txt의 절대 경로명
/home/chang/test/cs1.txt

cs1.txt의 상대 경로명
cs1.txt
```

### 디렉토리 관련 명령어
  - pwd : 현재 작업 디렉토리를 프린트
  - $ pwd
 
  - cd : 현재 작업 디렉토리를 이동
  - $ cd[____]
 
  - mkdir : 새 디렉토리를 만듬
  - $ mkdir ____
 
### 파일 관련 명령어
```
ls      | 파일 목록 보기                 
ls -l   | 상세 정보 보기                 
ls -a   | 숨김 파일 포함                 
ls -alt | 숨김 파일 포함 + 최신 파일 순서 
ls -lrt | 오래된 파일 → 최신 파일 순서    
```

### 파일 관련(ls)
```
ls -alt

-a | 숨김 파일 포함(.으로 시작)
-l | 상세 정보 표시                      
-t | 최근 수정 시간 순 정렬(최신 → 오래된) 
-----------------------------------------
ls -lrt

-l | 상세 정보 표시
-r | 역순 정렬
-t | 최근 수정 시간 순(오래된 → 최신)
-----------------------------------------
ls -lart
-l | 상세 정보 표시
-a | 숨김 파일 포함(.으로 시작)
-r | 역순 정렬
-t | 최근 수정 시간 순(오래된 → 최신)
```

### 파일 생성/삭제 관련 명령어
```
| 명령어 | 의미 | 예시 |
|-------|------|------|
| touch | 파일 생성 | touch test.txt |
| mkdir | 디렉토리 생성 | mkdir testdir | 
| rm | 파일 삭제 | rm test.txt |
| rm -r | 폴더 삭제 | rm -r file1 |
| rm -rf | 파일 하위 디렉토리 삭제 | rm -rf file1 | 
| cp | 파일 / 디렉토리 복사 | cp file1 file2 |
| mv | 파일 이동 / 이름 변경 | mv file1 backup/ |
--------------------------------------------------
rmdir : 디렉토리가 남아있을시 삭제 못함
rm -r : 아래 디렉토리 모두 삭제
```

### 파일 생성(touch)
- $ touch 파일이름
  - 파일이 없으면 새 파일 생성
  - 파일이 있으면 수정 시간 변경
```
파일 생성
$ touch file.txt

여러 파일 한 번에 생성
$ touch file1.txt file2.txt file3.txt

$ touch -t 202603202025 file.txt
→ 2026-03-20 20:25로 수정

$ touch -r file1.txt file2.txt
→ file1.txt 시간 = file2.txt 시간

로그 파일 미리 생성
$ touch server.log

여러 로그 파일 생성
$ touch log1.log log2.log log3.log
--------------------------------------
+ 개발자들이 많이 쓰는 패턴
$ mkdir project
$ cd project
$ touch README.md
```

### 디렉토리 생성(mkdir)
- mkdir 디렉토리이름
```
여러 디렉토리 한 번에 생성
$ mkdir dir1 dir2 dir3

상위, 하위 디렉토리 생성 (-p 옵션)
   - 중간 디렉토리까지 한 번에 생성 가능
$ mkdir -p project/src/java
project/
└── src/
└── java/

| 옵션 | 설명 |
|-----|------|
| -p | 상위, 하위 디렉토리까지 생성 |
| -m | 권한 설정 |
| -v | 생성 과정 출력 |

$ mkdir -pv project/src

- 출력 
mkdir: created directory 'project'
mkdir: created directory 'project/src'

$ tree
서버 디렉토리 다 보여줌
❌ 안될 경우 → $ sudo apt install tree → $ tree
- $ sudo apt install tree
  - sudo : 관리자 실행 권한
  - apt : 우분투에서 사용하는 패키지 관리 도구
  - install : 설치 명령어
```

### 파일 삭제(rm)
- 실수하면 복구가 어려울 수 있어 조심해서 사용해야 함
```
$ rm file.txt
→ file.txt 파일 삭제

$ rm file1.txt file2.txt file3.txt
→ 여러 파일 삭제

$ rm *.txt
→ 모든 파일 삭제
```

### 디렉토리 삭제(rm)
```
$ rm -r 폴더이름
→ 디렉토리 삭제하려면 -r 옵션 필요

$ rm -f file.txt
→ 강제 삭제(-f) : 삭제 확인 없이 강제로 삭제

$ rm -rf 폴더이름
→ 폴더 강제 삭제(-rf) : 매우 위엄한 명령어 💣

$ rm -i file.txt
→ 삭제 확인(i) : 삭제 전에 사용자에게 확인 
```

### 파일 삭제 / 폴더 삭제(rm)
```
| 옵션 | 설명 |
|-----|------|
| -r | 디렉토리 삭제 |
| -f | 강제 삭제 |
| -i | 삭제 확인 | 
| -v | 삭제 과정 표시 |
----------------------------------
+ 개발자가 가장 많이 쓰는 rm 형태
$ rm file.txt
$ rm *.log
$ rm -r folder
$ rm -rf build
$ rm -ir project
```

### 파일 복사(cp)
```
$ cp file1.txt file2.txt
file1.txt → file2.txt로 복사

다른 디렉토리로 복사
$ cp file.txt /home/user → file.txt를 /home/user/폴더로 복사 

여러 파일 복사
$ cp file1.txt file2.txt file3.txt bakcup/
```

### 디렉토리 복사(cp)
```
$ cp -r dir1 dir2
의미
dir1 폴더 전체 → dir2로 복사
(예시) $ cp -r project backup

$ cp -i file1.txt file2.txt
덮어쓰기 확인(-i)
파일이 이미 있을 때 확인 후 복사

$ cp -p file.txt backup/
파일 속성 유지(-p)
권한, 시간 등을 유지하면서 복사

| 옵션 | 의미 |
|-----|------|
| -r | 디렉토리 복사 |
| -i | 덮어쓰기 확인 |
| -v | 복사 과정 표시 |
| -p | 파일 속성 유지 |
| -u | 최신 파일만 복사 |

### 파일 이동 / 이름 변경(mv)

파일이나 디렉토리를 이동(move)하거나 이름을 변경(rename)할 명령어

$ mv old.txt new.txt
파일 이름 변경

$ mv file.txt /home/user
파일 이동

$ mv file1.txt file2.txt file3.txt backup/
$ mv *.txt backup/
여러 파일 이동
---------------------------------------------------
+ 실무에서 많이 쓰는 예시
$ mv *log logs/
로그 파일 정리

$ mv project project_old
프로젝트 백업

$ mv *.txt txt_files/
파일 정리
```

### 파일 내용 출력(cat)
- 파일 내용을 보여주거나 여러 파일을 이어 붙일 때 사용
```
$ cat file.txt
file.txt의 전체 내용이 터미널에 출력

$ cat file1.txt file2.txt
여러 파일 동시에 보기
- 결과
file1.txt 내용
file2.txt 내용 출력

파일 생성
$ cat > file.txt
입력 후
Hello
Linux
그리고
Ctrl + D
file.txt 파일이 생성되고 내용이 저장됨

파일 합치기
$ cat file1.txt file2.txt > new.txt
new.txt = file1.txt + file2.txt 내용

줄 번호 표시
$ cat -n new.txt ⇄ $ cat new.txt : 번호 없이 원래 내용 담겨있음
- 결과
1  Hello
2  Linux

빈 줄 번호 제외
$ cat -b new.txt
```

### 페이지 단위 보기(less)
- 페이지 단위로 편하게 읽는 명령어
  - 파일이 아주 길면 cat 쓰지 않고 less 많이 씀
  - → cat : 전체 출력(스크롤 지옥) | less : 페이지 단위로 보기
```
$ less file.txt
file.txt 내용을 한 화면씩 보여주면서 이동 가능

less 안에서 사용하는 키
| 키 | 기능 |
|----|------|
| Space | 다음 페이지 |
| b | 이전 페이지 |
| Enter | 한 줄 아래 |
| g | 파일 맨 위 |
| G | 파일 맨 아래 |
| /문자 | 검색 |
| n | 다음 검색 결과 |
| q| 종료 |
----------------------------
+ 실무에서 많이 사용
$ less /var/log/syslog
$ less /var/log/message
로그 파일 볼 때
서버에서 로그 확인할 때 거의 기본 명령어
```

### 파일 앞부분(몇줄) 확인(head)
```
$ head file.txt
기본적으로 처음 10줄을 보여줌
- 출력 예시
Line1
Line2
Line3
...
Line10

출력 줄 수 지정
$ head -n 5 file.txt
처음 5줄 보기
$ head 5 file.txt
파일 앞에서 5줄만 출력

$ head file1.txt file2.txt
여러 파일 동시에 보기
- 출력 예시
==> file1.txt <==
   (처음 10줄)

==> file2.txt <==
   (처음 10줄)
---------------------------
+ 실무 예시
$ head /var/log/syslog
로그 파일 처음 확인
$ head data.csv
CSV데이터 구조 확인
데이터 파일 컬럼 구조 확인할 때 많이 사용
```

### 파일 끝부분을 확인(tail)
- 마지막 줄들을 보는 명령어
  - 로그 파일 확인할 때 많이 쓰임
  - 서버 작업할 때 거의 필수
```
$ tail file.txt
기본적으로 마지막 10줄을 출력
- 예시
Line91
Line92
Line93
...
Line100

출력 줄 수 지정
$ tail -n 5 file.txt 🅾️ → $ tail 5 file.txt ❌
마지막 5줄 보기
------------------------------------------------
+ 실무 예시
$ tail -f log.txt
-f = follow
실시간 로그 보기

$tail -f /var/log/syslog
파일에 새로운 내용이 추가될 때마다 계속 출력
(예시) 서버 로그 모니터링

종료 할 때 Ctrl + C
```

### 두 번째 터미널 열기
```
같은 폴더에서 실행
$ echo "first log message" >> log.txt
그러면 첫 번째 터미널에서 바로 출력
first log message
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-03-20
