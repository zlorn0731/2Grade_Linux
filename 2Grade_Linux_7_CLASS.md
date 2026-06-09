# 👨‍💻리눅스와 유닉스

## 8장 파일 다루기와 프로세스

### 파일 내용 검색하기 (실습)
- 실습을 위한 sample 파일을 만든다
```
Hello World
Ubuntu
Rocky Linux
Good
morning
afternoon
evening
what??
monday
sunday
```
- oo가 들어간 문자열을 검색하여 행 번호와 함께 출력한다
```
grep -n "oo" sample
```
- g로 끝나는 문자열을 검색한다
```
grep "g$" sample
```
- day 앞에 임의의 세 글자가 나오는 문자열을 검색한다
```
grep "...day" sample
```
- 대문자 R이나 G 다음에 o가 나오는 문자열을 검색한다
```
grep "[RG]o" sample
```
- i가 1회 이상 나오는 문자열을 검색한다
```
grep "i￦+" sample
grep -E "i+" sample
```
- morn 또는 even 패턴이 나오는 문자열을 검색한다
```
grep -E "monr|even" sample
```
- * : 0회 이상
  + : 1회 이상
  | : OR

### 파일 정렬과 중복 제거
- 파일 내용 정렬 : sort
  - sort 명령은 텍스트 파일의 내용을 정렬하여 화면으로 출력함
  - 형식 : sort [옵션]파일
  - 옵션
    - -f : 대손문자를 구분하지 않고 정렬
    - -k# : #에 지정한 필드를 기준으로 정렬
    - -r : 역순으로 정렬
    - -u : 정렬 후에 중복된 내용을 제거
    - -o 파일명 : 정렬 결과를 지정한 파일에 저장
    - -t 문자 : 지정한 문자를 필드 구분자로 사용
  - 사용 예
    - sort sample
    - sort -d sample
    - sort -o sort.out sample
- sort 명령의 실습
  - 기본 정렬(sort)
```
$ cp sample sort.dat
$ vi sort.dat
game
game
apple
ball
people
taste
linux
Linux
12345
우분투
:wq
```
- sort 명령을 옵션 없이 사용하려면 숫자 → 영어 대문자 → 영어 소문자 → 한글 순서로 정렬됨
```
$ sort sort.dat
12345
Linux
apple
ball
game
game
linux
people
taste
우분투
```
- -f 옵션을 사용하면 대소문자를 구분하지 않고 정렬
- 기본 정렬과 비교하면 Linux와 linux의 위치가 변경됨
```
$ sort -f sort.dat
12345
apple
ball
game
game
Linux
linux
people
taste
우분투
```
- -r 옵션을 사용하면 기본 정렬 순서의 역순으로 정렬
- 한글 → 소문자 → 대문자 → 숫자 순서로 정렬됨
```
$ sort -r sort.dat
우분투
taste
people
linux
game
game
ball
apple
Linux
12345
```
- -u 옵션을 사용하면 중복된 내용을 제거하고 정렬된 결과를 출력
- 중복된 game이 하나만 출력됨
```
$ sort -u sort.dat
12345
Linux
apple
ball
game
linux
people
taste
우분투
```
- -o 옵션을 사용하면 정렬된 결과를 지정한 파일에 저장
```
$ sort -o sort.out sort.dat
$ cat sort.out
12345
Linux
apple
ball
game
game
linux
people
taste
우분투
```
- 중복 내용 제거 : uniq
  - uniq 명령은 연속으로 중복된 행을 하나로 합병하거나 중복 여부를 확인
  - 형식 : uniq [옵션] 파일 [결과 파일]
  - 옵션
    - -c : 중복 횟수를 각 행의 앞부분에 표시
    - -d : 중복된 행만 한 행을 출력
    - -i : 중복 여부를 비교할 때 대소문자를 구분하지 않음
    - -u : 중복되지 않은 행만 출력
  - 사용 예
    - uniq sample
    - uniq -C sample
- uniq 명령을 실습하기 위해 sample 파일을 uniq.dat 파일로 복사하고 수정
```
$ cp sample uniq.dat
$ vi uniq.dat
game
game
apple
ball
people
book
tomato
game
linux
Linux
:wq
```
- uniq 명령을 옵션 없이 실행하면 연속된 중복 행만 제거되고, 중복되지 않은 행만 출력됨
```
$ uniq uniq.dat
game
apple
ball
people
book
tomato
game
linux
Linux
```
- -c 옵션을 사용하면 각 행 앞에 중복 횟수를 표시
- 중복된 행이 연속으로 몇 번 나타나는지 확인 가능
```
$ uniq -c uniq.dat
  2 game
  1 apple
  1 ball
  1 people
  1 book
  1 tomato
  1 game
  1 linux
  1 Linux
```
- -d 옵션을 사용하면 연속으로 중복된 행만 출력
```
$ uniq -d uniq.dat
game
```
- -u 옵션을 사용하면 중복되지 않은 행만 출력
```
$ uniq -u uniq.dat
apple
ball
people
book
tomato
game
linux
Linux
```
- -i 옵션을 사용하면 대소문자를 구분하지 않고 중복 여부를 판단
- 같은 단어이지만 대소문자가 다른 경우 중복으로 처리됨
```
$ uniq -i uniq.dat
game
apple
ball
people
book
tomato
game
linux
$ uniq -di uniq.dat
game
linux
```
- 파이프(|)를 사용하면 여러 명령어를 연결하여 실행 가능
```
$ sort uniq.dat | uniq
Linux
apple
ball
book
game
linux
people
tomato
```

### 파일 내용 비교
- 파일의 내용을 비교할 때 사용할 수 있는 명령 : diff, diff3, cmp
- 두 파일 내용 비교 : diff
  - diff 명령은 두 파일의 내용을 행 단위로 비교하여 차이점을 출력
  - 형식 : diff [옵션] 파일1 파일2
  - 옵션
    - -q : 두 파일에 다른 부분이 있는지만 간단하게 출력
    - -c# : 두 파일의 비교 결과를 #에 지정한 행만큼 출력
    - -r : 디렉토리의 내용을 비교
    - -i : 대소문자를 구분하지 않고 비교
  - 사용 예
    - diff sample sample2
    - diff -q sample sample2
    - diff -r temp temp2
- diff 명령 실습을 위해 sample 파일을 diff.dat로 복사하고 내용 수정
```
$ cp sample diff.dat
$ vi diff.dat
Game
apple
ball
people
book
tomato
taste
linux
Linux
ubuntu
grape
what?
:wq
```
- diff 명령을 옵션 없이 실행하면 두 파일 간의 차이점을 출력
```
$ diff sample diff.dat
1c1
< game
---
> Game
```
- diff.dat 파일에 2행에 school을 추가한 후 diff 명령으로 비교하면 추가된 내용이 출력됨
```
$ vi diff.dat
Game
School
apple
(생략)
:wq

$ diff sample diff.dat
1c1, 2
< game
---
> Game
> School
```
- -q 옵션을 사용하면 두 파일이 다름을 나타내는 간단한 메시지만 출력
```
$ diff -q sample diff.dat
파일 sample와(과) diff.dat이(가) 다릅니다
```
- -c 옵션을 사용하면 파일의 최종 수정 시간과 함께 차이점 및 일부 동일한 내용을 출력
- 기본적으로 3행씩 출력되며, 다른 부분에는 ! 표시
```
$ diff -c sample diff.dat
*** sample 최종 수정 시간
--- diff.dat 최종 수정 시간
************
*** 1, 4 ***
! game
  apple
  ball
  people
---1, 5 ---
! Game
! school
  apple
  ball
  people
```
- 세 파일 내용 비교 : diff3
  - 형식 : diff3 파일1 파일2 파일3
  - 사용 예
    - diff3 sample sample2 sample3
- diff3 명령을 실습하기 위해 diff.dat 파일을 diff3.dat로 복사하고, 2행의 school을 SCHOOL로 수정
```
$ cp diff.dat diff3.dat
$ vi diff3.dat
Game
SCHOOL
apple
(생략)
:wq
```
- diff3 명령을 사용하여 sample, diff.dat, diff.dat 파일을 비교
```
$ diff3 sample diff.dat diff3.dat
====
1:1c
  game
2:1,2c
  Game
  school
3:1,2c
  Game
  SCHOOL
```

### 프로세스의 이해
- 프로세스의 개념
  - 프로세스는 현재 시스템에서 실행 중인 프로그램을 의미
  - 리눅스는 다중 프로세스 시스템으로, 여러 프로세스가 동시에 실행됨
  - 시스템 프로세스는 리눅스 시스템의 운영에 필요한 기능을 수행
  - 사용자 프로세스는 사용자가 실행한 명령이나 프로그램을 처리
```
                 [리눅스 시스템]
  [시스템 프로세스]             [사용자 프로세스]
      [Systemd]                 [sort]   [vi]
  [Network Manager]             [cp]   [find]
       [sshd]
```
- sort 명령을 옵션 없이 사용하면 숫자 → 영어 대문자 → 영어 소문자 → 한글 순서로 정렬됨
```
$sort sort.dat
12345
Linux
apple
ball
game
game
linux
people
taste
우분투
```
- -f 옵션을 사용하면 대소문자를 구분하지 않고 정렬
- 기본 정렬과 비교하면 Linux와 linux의 위치가 변경됨
```
$ sort -f sort.dat
12345
apple
ball
game
game
Linux
linux
people
taste
우분투
```
- -r 옵션을 사용하면 기본 정렬 순서의 역순으로 정렬
- 한글 → 소문자 → 대문자 → 숫자 순서로 정렬됨
```
$ sort -r sort.dat
우분투
taste
people
linux
game
game
ball
apple
Linux
12345
```
- -u 옵션을 사용하면 중복된 내용을 제거하고 정렬된 결과를 출력
- 중복된 game이 하나만 출력됨
```
$ sort -u sort.dat
12345
Linux
apple
ball
game
linux
people
taste
우분투
```
- -o 옵션을 사용하면 정렬된 결과를 지정한 파일에 저장
```
$ sort -o sort.out sort.dat
$ cat sort.out
12345
Linux
apple
ball
game
game
linux
people
taste
우분투
```

### 프로세스의 생성과 부모-자식 관계
- 리눅스에서 프로세스는 부모-자식 관계를 가짐
- 시스템 프로세스인 systemd와 kthreadd는 모든 프로세스의 조상
- 사용자 프로세스는 사용자가 명령을 실행할 때 생성되며, 부모 프로세스는 자식 프로세스를 생성할 수 있음
- 예시 : 사용자가 find명령을 입력하면 셸이 부모 프로세스가 되어 자식 프로세스로서 find 명령을 실행
- 자식 프로세스는 작업을 완료하고 부모 프로세스에 결과를 반환한 후 종료
```
[부모 프로세스] (예시 : 배시셸은 자식 프로세스를 생성)
 생성↓   ↑복귀
[자식 프로세스] (예시 : 배시셸에 의해 vi, cp, find 등이 실행)
```
- 프로세스의 번호
  - PID(프로세스 ID)는 리눅스 시스템에서 각 프로세스를 고유하게 식별하는 번호
  - PID는 1번부터 시작하여 프로세스가 실행되면서 하나씩 증가
  - PID 1번은 systemd 프로세스로, 모든 시스템 프로세스의 부모 프로세스
  - PID 2번은 kthreadd 프로세스로, 모든 스레드의 부모 프로세스
- 특별한 프로세스
  - 데몬 프로세스
    - 특정 서비스를 제공하는 프로세스로, 평소대로 대기 상태에 있다가 서비스 요청 시 실행됨
    - 예시 : sshd(원격 접속 서비스), httpd(웹 서버 서비스)
    - 리눅스 시스템 설치 시 기본적으로 제공되며, 시스템 관리자가 추가 설치 가능
- 고아 프로세스
  - 자식 프로세스가 실행 중인데 부모 프로세스가 종료되면 발생
  - 부모가 종료되면 PID 1번 고아 프로세스의 새로운 부모가 되어 종료할 수 있도록 함
- 좀비 프로세스
  - 좀비 프로세스는 자식 프로세스가 종료했지만, 부모 프로세스가 종료 정보를 읽지 않아서 프로세스 목록에 남아 있는 상태
  - defunct 프로세스로도 표시되며, 실제로 실행 중이지 않음
  - 좀비 프로세스는 프로세스 테이블을 차지하고 있기 때문에 많아지면 시스템 자원이 부족해질 수 있음

### 프로세스 관리 명령
- 프로세스 정보 검색 명령 : ps
  - 기능 : 현재 실행 중인 프로세스에 대한 정보를 출력
  - 형식 : ps [옵션]
  - 옵션
    - -e : 시스템에서 실행 중인 모든 프로세스의 정보를 출력
    - -f : 프로세스에 대한 자세한 정보를 출력
    - -l : 실행 우선 순위 값인 nice를 포함하여 출력
    - -u uid : 특정 사용자에 대한 모든 프로세스의 정보를 출력
    - -p pid : pid로 지정한 특성 프로세스의 정보를 출력
    - a : 터미널에서 실행시킨 프로세스의 정보를 출력
    - u : 프로세스 소유자 이름, CPU 사용량, 메모리 사용량 등 상세 정보를 출력
    - x : 터미널과 연계되지 않은 프로세스의 정보도 포함하여 출력
  - 사용 예
    - ps
    - ps -ef
    - ps aux
- 옵션 없이 사용하기
  - 옵션 없이 ps 명령을 사용하면 현재 셸이나 터미널에서 실행한 사용자 프로세스의 정보를 출력
- 
| 항목 | 의미 |
|------|------|
| PID | 프로세스 번호 |
| TTY | 프로세스를 실행한 터미널 번호 |
| TIME | 해당 프로세스가 사용한 CPU 시간의 양 |
| CMD | 프로세스가 실행 중인 명령 |
  - 프로세스의 PID는 실행 환경에 따라 달라지며, 각 프로세스가 사용하는 CPU 시간은 매우 짧음
```
$ ps
```
- SVR4 옵션 사용하기
  - 프로세스 상세 정보 출력하기 : -f 옵션
  - 
  | 항목 | 의미 | 항목 | 의미 |
  |------|-----|------|------|
  | UID | 프로세스를 실행한 사용자 ID | STIME | 프로세스의 시작 날짜나 시간 |
  | PID | 프로세스 번호 | TTY | 프로세스가 실행된 터미널의 종류와 번호 |
  | PPID | 부모 프로세스 번호 | TIME | 프로세스 실행 시간 |
  | C | CPU 사용량(% 값) | CMD | 실행되고 있는 프로그램 이름(명령) |
  - 출력 결과에는 PID뿐만 아니라 부모-자식 관계도 포함되어 부모 프로세스(PPID)의 PID도 확인할 수 있음
  ```
  $ ps -f
  ```
- 전체 프로세스 목록 출력하기 : -e 옵션
  - -e 옵션은 시스템에서 실행 중인 전체 프로세스를 출력
  - TTY 값이 ?인 프로세스는 대부분 시스템에서 실행된 프로세스
  - 전체 프로세스를 한 번에 출력하면 너무 많을 수 있기 때문에, | more 또는 | less 를 사용하여 페이지 단위로 결과를 확인할 수 있음
  ```
  $ ps -e | more
  ```
  - -e 옵션과 -f 옵션을 함께 사용하면 전체 프로세스와 세부 정보를 출력
  - 스레드는 CMD 필드에 []로 표시하여 구분
  ```
  $ ps -ef | more
  ```
- 특정 사용자의 프로세스 목록 출력하기 : -u 옵션
  - -u 옵션을 사용하면 특정 사용자가 실행한 프로세스 목록을 출력할 수 있음
  ```
  $ ps -u user1 | more
  ```
- 프로세스 상세 정보 출력하기 : u 옵션
  - u 옵션을 사용하면 터미널에서 실행한 프로세스의 상세 정보를 출력
  - a 옵션이나 -f 옵션을 사용한 것보다 CPU 사용량, 메모리 사용량 등 추가적인 정보가 포함됨
  ```
  $ ps u
  ```
  - 
  | 항목 | 의미 |
  |------|-----|
  | USER | 사용자 계정 이름 |
  | %CPU | 퍼센트로 표시한 CPU 사용량 |
  | %MEN | 퍼센트로 표시한 물리적 메모리 사용량 |
  | VSZ | 사용 중인 가상 메모리의 크기(KB) |
  | RSS | 사용 중인 물리적 메모리의 크기(KB) |
  | START | 프로세스 시작 시간 |
  - a 옵션을 함께 사용하면 시스템에서 실행 중인 모든 프로세스에 대한 정보를 출력
  ```
  $ ps aux
  ```
- 특정 프로세스 정보 검색하기 : ps와 grep
  - ps와 grep 명령 연결하여 검색하기
    - ps 명령과 grep 명령을 |(파이프)로 연결하여 특정 프로세스를 검색할 수 있음
    - ps -ef | grep 형태로 연결하여 시스템에서 실행 중인 프로세스 중 원하는 프로세스를 검색
    ```
    $ ps -ef | grep bash
    ```
- 프로세스 종료 : kill
  - kill 명령은 지정한 프로세스를 종료하는 명령
  - 형식 : kill [-시그널] PID...
  - 시그널 9 : 프로세스를 강제로 종료
  - 사용 예
    - kill 1001
    - kill -9 1001
- 프로세스 관리 도구 : top
  - top 명령은 현재 실행 중인 프로세스의 정보를 주기적으로 출력하는 도구
  - 프로세스의 요약 정보는 상단에, 각 프로세스의 세부 정보는 하단에 출력
  - top 명령은 시스템 자원 사용 현황을 실시간으로 확인할 수 있어 프로세스 모니터링에 매우 유용함
  - 
  | 항목 | 의미 |
  |------|-----|
  | PID | 프로세스 ID |
  | USER | 사용자 계정 |
  | PR | 우선 순위 |
  | NI | Nice 값 |
  | VIRT | 프로세스가 사용하는 가상 메모리의 크기 |
  | RES | 프로세스가 사용하는 메모리의 크기 |
  | SHR | 프로세스가 사용하는 공유 메모리의 크기 |
  | %CPU | 퍼센트로 표시한 CPU 사용량 |
  | %MEN | 퍼센트로 표시한 메모리 사용량 |
  | TIME+ | CPU 누적 이용 시간 |
  | COMMAND | 명령 이름 |
  ```
  $ top
  ```

### 작업 제어
- 작업 제어(Job Control)는 한 터미널에서 동시에 여러 프로세스를 실행하고 관리할 수 있게 해주는 기능
- 작업 제어는 C셸에서 처음 개발되었고, 이후 대부분의 셸에 포함되어 제공
- 작업은 포그라운드(foreground)작업과 백그라운(background)작업으로 나뉘며, 이를 통해 사용자는 여러 명령을 동시에 실행하고 관리할 수 있음
- 포그라운드 작업
  - 포그라운드 작업은 사용자가 입력한 명령이 실행되어 결과를 화면에 출력할 때까지 기다리는 방식으로 처리되는 프로세스
  - 사용자가 입력한 명령이 실행되는 동안 셸은 결과를 출력하기 위해 기다리며, 명령이 종료될 때까지 터미널을 사용할 수 없음
  ```
  $ sleep 1000 → 포그라운드 작업(포그라운드 프로세스)
               → sleep 명령이 끝날 때까지 기다림
  ```
- 백그라운드 작업
  - 백그라운드 작업은 명령이 실행될 때 처리가 끝나는 것과 관계없이 바로 프롬포트가 출력되어 사용자가 다른 작업을 계속할 수 있는 방식
  - 백그라운드 프로세스는 명령이 실행되며 터미널을 차지하지 않으므로 다른 작업을 계속할 수 있음
  - 여러 작업을 백그라운드로 실행하여 터미널에서 포그라운드 작업을 진행할 수 있음
  - 백그라운드 작업을 실행하려면 명령어의 끝에 &기호를 추가함
  ```
  $ sleep 100&    → 백그라운드 작업(백그라운드 프로세스)
  [1] 105730      
  user1@hanbit:~$ → 바로 프롬포트가 나와 다른 작업을 할 수 있음
  ```
- 작업 목록 보기 : jobs
  - jobs 명령은 현재 실행 중인 백그라운드 작업을 확인하는 명령
  - 형식 : jobs [%작업 번호]
  - %작업 번호
    - %작업 번호 : 해당 번호의 작업 정보를 출력
    - %+ 또는 %% : 작업 순서가 +인 작업 정보를 출력
    - %- : 작업 순서가 -인 작업 정보를 출력
  - 사용 예
    - jobs %1
    - jobs
- sleep 명령을 백그라운드로 실행하고 jobs 명령으로 확인한 예
```
$ sleep 100&
[1] 106041
$ jobs
[1]+    실행중      sleep 100&
```

- 작업 전환하기
  - 작업 전환은 포그라운드 작업을 백그라운드 작업으로 전환하거나, 반대로 백그라운드 작업을 포그라운드 작업으로 전환하는 작업
  - 
  | 명령 | 기능 |
  |------|-----|
  | Ctrl + Z | 포그라운드 작업을 중지한다(종료가 아닌 잠시 중단) |
  | bg %작업 번호 | 작업 번호가 지시하는 작업을 백그라운드 작업으로 전환 |
  | fg %작업 번호 | 작업 번호가 지시하는 작업을 포그라운드 작업으로 전환 |

- 포그라운드 작업을 백그라운드 작업으로 전환하기
  - 포그라운드 작업을 백그라운드 작업으로 전환하려면 먼저 해당 작업을 중지시켜야함
  - Ctrl + Z를 사용하여 작업을 일시 중지하고, 이후 bg %작업 번호 명령을 사용하여 백그라운드로 전환할 수 있음
  ```
  $ jobs                            → 현재 백그라운드 작업이 없음
  $ sleep 100                       → 포그라운드로 실행
  ^Z                                → Ctrl + Z로 일시 중지   
  [1]+   멈춤      sleep 100        → 일시 중지된 상태
  $ bg %1                           → 백그라운드로 전환
  [1]+   sleep 100 &
  $ jobs
  [1]+   실행중    sleep 100 &      → 백그라운드 실행 중
  ```
- 백그라운드 작업을 포그라운드 작업으로 전환하기
  - 백그라운드 작업을 포그라운드 작업으로 전환하려면 fg %작업 번호 명령을 사용
  - 작업 번호를 지정하지 않으면, fg 명령은 가장 최근에 실행된 백그라운드 작업(작업 순서가 +인 작업)에 적용
  ```
  $ jobs
  [1]+   실행중        sleep 100 &    → 백그라운드로 실행 중
  $ fg                               → 포그라운드로 전환
  sleep 100                          → 포그라운드로 실행 중
  ```

- 작업 종료하기
  - 일반적으로 Ctrl + C를 입력하면 포그라운드 작업이 종료
  - 다른 터미널에서 해당 프로세스의 PID를 검색하여 강제 종료할 수 있음
  ```
  $ sleep 100          → 포그라운드 실행 중
  ^C                   → 강제 종료
  ```
  - kill 명령을 사용하여 백그라운드 작업을 강제 종료할 수 있음
  - 이때 PID 대신 %작업 번호를 지정하여 종료할 수 있음
  ```
  $ sleep 100&                      → 백그라운드로 실행 중
  [1] 106082
  $ kill %1                         → 강제 종료
  $ jobs          
  [1]+   종료됨      sleep 100      → 종료 메시지가 출력
  ```

- 로그아웃 후에도 백그라운드 작업 계속 실행하기 : nohup
  - nohup 명령은 백그라운드 작업을 실행 중인 터미널이 종료되거나 사용자가 로그아웃해도 작업이 계속 실행되도록 해주는 명령
  - nohup은 터미널 세션 종료와 관계없이 작업을 백그라운드에서 계속 실행하도록 보장
  - 형식 : nohup 명령&

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-09
