# 리눅스 2학년 1학기 시험 범위 공부

## 1장

### 커널
- 하드웨어 운영 및 관리
- 컴퓨터 자원을 초기화하고 제어
### 쉘
- 사용자와 운영체제 사이의 인터페이스
- 명령어 해석기

## 2장

### 간단한 명령어
- $ date : 현재 시각
- $ hostname : 플랫폼 이름
- $ uname : 현재 설치된 OS
- $ who : 아이디
- $ clear : 화면 기록 삭제
- $ history : 과거에 썼던 명령어들
- $ pwd : 현재 위치
- $ cd : 디렉토리 이동
- $ cd / : root 디렉토리
- $ ls : 조회
- $ mkdir : 디렉토리 만듦
- $ touch : 파일 만듦


### 리눅스 디렉토리
- /bin : 기본 명령어
- /sbin : 부팅관련 명령어
- /etc : 환경설정 파일
- /usr : 명령어, 시스템 프로그램, 라이브러리 루틴
- /boot : 리눅스 커널 이미지 부트로더
- /dev : 디바이스 파일
- /home : 홈디렉토리
- /root : 관리자 홈디렉토리
- /lib : 라이브러리

## 3장

### 연습문제
- 문제 1
```
-rwxr-xr--?
소유자는 읽고쓰고실행 가능 그룹은 읽고실행만 가능 다른 사용자들은 읽기만 가능
숫자로 하면 754
```
- 문제 2
```
rwxr-xr-x 권한을 설정하는 명령어 작성
chmod 755 파일명
```
- 문제 3
```
linux 디렉토리 생성
file.txt 생성
권한을 644로 설정
실행 권한 추가

mkdir linux
touch file.txt
chmod 644 file.txt
chmod +x file.txt
```

### 실습
- 문제 1
```
현재 작업 디렉토리를 확인하시오
pwd
```
- 문제 2
```
home디렉토리로 이동하시오
cd ~
```
- 문제 3
```
다음 디렉토리를 생성하시오 linux
mkdir linux
```
- 문제 4
```
linux 디렉토리 안에 study, practice디렉토리를 생성하시오
cd linux
mkdir study practice
```
- 문제 5
```
study디렉토리로 이동하시오
cd study
```
- 문제 6
```
다음 파일을 생성하시오
file1.txt
file2.txt
file3.txt
touch file1.txt file2.txt file3.txt
```
- 문제 7
```
현재 디렉토리의 파일 목록을 상세 정보와 함께 출력하시오
pwd
ls -lrt
```
- 문제 8
```
숨김 파일까지 포함하여 파일 목록을 출력하시오
ls -lart
```
- 문제 9
```
file1.txt파일을 복사하여 file1_backup.txt파일을 생성하시오
cp file1.txt file1_backup.txt
```
- 문제 10
```
file2.txt파일을 practice 디렉토리로 이동하시오
mv file2.txt practice
```

### 권한실습
- 문제 1
```
다음 파일을 생성하시오 script.sh
touch script.sh
```
- 문제 2
```
script.sh파일에 실행 권한을 추가하시오
chmod +x script.sh
```
- 문제 3
```
script.sh파일 권한을 다음과 같이 변경하시오 rwxr-xr-x
chmod 755 script.sh
```
- 문제 4
```
test.txt파일 권한을 다음과 같이 변경하시오 rw-r--r--
chmod 644 test.txt
```
- 문제 5
```
다음 디렉토리를 생성하시오 secure
권한을 700으로 설정하시오
mkdir secure
chmod 700 secure
```
- 문제 6
```
secure디렉토리 권한을 다음과 같이 변경하시오 rwxr-x---
chmod 750 secure
```
- 문제 7
```
script.sh파일에서 others의 실행 권한을 제거하시오
chmod o-x script.sh
```
- 문제 8
```
test.txt파일에 group쓰기 권한을 추가하시오
chmod g+w test.txt
```
- 문제 9
```
project디렉토리와 모든 하위 파일의 권한을 755로 변경하시오
chmod -R 755 project
```
- 문제 10
```
test.txt파일의 소유자를 root로 변경하시오
chown root test.txt
```

## 4장

### 실습
- 실습 1:파일 생성
```
vi test.txt
```
- 실습 2:입력 모드
```
i
```
```
내용 입력

Hello Linux
```
- 실습 3:저장
```
:w
```
- 실습 4:종료
```
:q
```
- 실습 5:한 줄 삭제
```
:dd
```
- 실습 6:복사&붙여넣기
```
yy
p
```
- i : 입력
- a : 다음 커서로 입력
- ESC : 명령모드
- :wq : 저장하고 종료
- dd : 한 줄 삭제
- yy : 줄 복사
- p : 붙여넣기
- D : 한 행 삭제
- x : 한단어 삭제
- G : 맨 위로
- g : 맨 아래로
- u : 명령 취소
- r : 한 글자 수정
- cw : 여러 글자 수정

## 5장

### 셸 기본 사용법 실습
- 셸 변수에 HOSTNAME이 설정되어 있는지 확인한다
```
echo $HOSTNAME
```
- 환경 변수에 TERM이 설정되어 있는지 확인한다
```
echo $TERM
```
- 환경 변수 LANG에 설정된 값을 출력한다
```
echo $LANG
```
- 셸 변수 HAN의 값을 han으로 설정한다
```
HAN=han
```
- 환경 변수 BIT의 값을 bit로 설정한다
```
BIT=bit
```
- 셸 변수 HAN의 값을 확인한다
```
echo $HAN
```
- 환경 변수 BIT의 값을 확인한다
```
echo $BIt
```
- 변수 HAN과 BIT의 설정을 해제한다
```
unset HAN BIT
```

## 5장

### 앨리어스(alias)와 히스토리(history)
- alias
  - 긴 명령어를 짧은 별칭으로 지정하는 것
```
alias 이름='명령'

alias h='history'
```
- alias 삭제
```
unalias h
```
- history
  - 이전 명령 보기

### 앨리어스와 히스토리 실습
- 문제 1 : pwd명령과 ls명령을 묶어서 앨리어스 pls를 만든다
```
alias pls='pwd; ls'
```
- 문제 2 : history명령을 앨리어스 h로 만든다
```
alias h='history'
```
- 문제 3 : 앨리어스 pls를 삭제한다
```
unalias pls
```

### 작업 제어(sleep)
- 지정된 시간만큼 실행을 중단
```
$ sleep 초
```

### 작업 제어(kill)
- 현재 실행중인 프로세스를 강제로 종료
```
$ kill [-시그널] 프로세스 번호
```

### 작업 제어(wait)
- 백그라운드로 실행된 프로세스가 끝날 때까지 기다리는 명령어

## 6장

### 비교 연산자
- 같다 : -eq == 
- 크다 : -gt >
- 작다 : -lt <
- 같지 않다 : -ne !=
- 작거나 같다 : -le <=
- 크거나 같다 : -ge >=

### 조건문
```
read num
if [ $num -gt 10 ]
then
    echo "10보다 큼"
else
    echo "10보다 작음"
fi
```
```
read score
if [ $score -ge 60 ]
then 
    echo "합격"
else
    echo "불합격"
fi
```

### 반복문
```
for i in 1 2 3
do
   echo $i
done
```
```
i=1
while [ $i -le 5 ]
do
   echo $i
   i=$((i+1))
done
```

### 반복문 실습
```
#!/bin/bash

echo "이름 입력:"
read name
echo "숫자 입력:"
read num

if [ $num -gt 10 ]
then
   echo "$name 님, 숫자가 큽니다"
else
   echo "$name 님, 숫자가 작습니다"
fi


for i in {1..3}
do
   echo "Hello $name"
done
```

### 파일 처리
- -f : 파일 존재
- -d : 디렉토리
- -e : 존재 여부

### 함수+변수
```
#!/bin/bash

print_name() {
  echo "My name is $name"
}

name="Kim"
print_name
```

### 함수+입력값
```
#!/bin/bash

sum() {
   result=$((a+b))
   echo "합: $result"
}

a=10
b=20
sum
```

### 함수+파라미터
```
#!/bin/bash

add() {
   echo "합: $(($1+$2))
}
add 10 20
```

### 예제 2 : 짝수 출력
```
#!/bin/bash

for i in {1..10}
do
   if [ $((i%2)) -eq 0 ]
   then
       echo $i
   fi
done
```

### 예제 3 : 파일 존재 확인
```
#!/bin/bash

read filename

if [ -f $filename ] 
then 
    echo "파일 존재"
else
    echo "파일 없음"
fi
```

### 실습 문제
- hello.sh 파일을 생성하고 "Hello Linux"를 출력하는 스크립트를 작성하시오
```
vi hello.sh

#!/bin/bash

echo "Hello Linux"

chmod +x hello.sh
./hello.sh
```
- 현재 날짜와 시간을 출력하는 스크립트를 작성하시오
```
#!/bin/bash

date
```
- 현재 사용자 이름을 출력하는 스크립트를 작성하시오
```
#!/bin/bash

whoami
```
- 현재 작업 디렉토리를 출력하는 스크립트를 작성하시오
```
#!/bin/bash

pwd
```
- 변수 name에 본인 이름을 저장하고 출력하시오
```
#!/bin/bash

name="김성결"
echo $name
```
- 두 숫자를 변수에 저장하고 합을 출력하시오
```
#!/bin/bash

a=10
b=20
sum=$((a+b))
echo $sum
```
- 사용자로부터 이름을 입력받아 출력하는 스크립트를 작성하시오
```
#!/bin/bash

echo "이름을 입력: "
read name
echo $name
```
- 사용자로부터 숫자를 입력받아 2배 값을 출력하시오
```
#!/bin/bash

echo "숫자를 입력: "
read num
echo $((num * 2))
```
- 현재 날짜를 변수에 저장하여 출력하시오
```
#!/bin/bash

today=$(date)
echo $today
```
- 입력받은 숫자가 10보다 크면 "크다"를 출력하시오 -gt
```
#!/bin/bash

read num

if [ $num -gt 10 ]
then
    echo "크다"
fi
```
- 입력받은 숫자가 짝수인지 홀수인지 출력하시오
```
#!/bin/bash

read num

if [ $((num%2)) -eq 0 ]
then
   echo "짝수"
else
   echo "홀수"
fi
```
