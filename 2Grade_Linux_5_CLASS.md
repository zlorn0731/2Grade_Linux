# 👨‍💻리눅스와 유닉스

## 6장 쉘 스크립트(shell script)

### 쉘 스크립트
- 리눅스 자동화를 위한 가장 강력한 도구
- 즉, 명령어를 자동으로 실행하는 프로그램
```
1. 에디터를 사용하여 Bash 스크립트 파일을 작성
#! = "이 스크립트의 실행 엔진"
#!/bin/bash : 이 스크립트를 bash로 실행하라

// file name: name.bash
#!/bin/bash
name="Kim"
echo $name

2. chmod를 이용하여 실행 모드로 변경
$ chmod +x name.bash

3. 스크립트 이름을 타입핑하여 실행
$ ./name.bash
```

### 기본 변수(문자열)
- 하나의 값(문자열)만을 저장할 수 있는 변수
- 따옴표 없어도 가능
```
$ 이름=단어
$ city=seoul
```
- 변수의 값 사용
```
$ echo $city
seoul
```
- 변수에 어느 때나 필요하면 다른 값을 대입
```
$ city=pusan
```
- 한 번에 여러 개의 변수를 생성
```
$ country=korea city=seoul
```

#### 문자열 실습
```
$ vi str.sh
$ #!/bin/bash
$ country=Korea city=Seoul
$ echo $country $city
$ chmod +x str.sh
$ ./str.sh
Korea Seoul

공백이 있으면 반드시 ""필요
$ address="Seoul City Gangnam Gu"
```

### 숫자 변수(정수)
- 실제로는 문자열이지만 연산할 때 숫자로 처리
- 숫자 타입 따로 없음 → $(())로 계산할 때 숫자 취급
```
$ vi ari.sh
$ #!/bin/bash
$ a=10
$ b=5
$ echo $((a+b))
$ chmod +x ari.sh
$ ./ari.sh
15
```

### 정수 변수(실수)
- Bash는 실수 연산을 직접 지원하지 않음
  - 외부 도구를 사용해야 함
```
echo $((5/2))
결과는 2이며 2.5아님
Bash는 정수 연산만 지원

$ vi float.sh
$ #!/bin/bash
$ a=5
$ a=2
$ result=$(echo "scale=2; $a/$b" | bc) // bc : 소수점 두 번째 자리까지
$ echo $result
$ chmod +x float.sh
$ ./float.sh
2.50
```

### 배열(Array)
- 한 변수에 여러 개의 값(문자열)을 저장할 수 있는 변수
```
$ 이름=(단어리스트)
$ city=(seoul busan daegu)
```
```
$ vi array.sh
$ #!/bin/bash
$ arr=(apple banana cherry)
$ echo ${arr[0]}
$ echo ${arr[1]}
$ chmod +x array.sh
$ ./array.sh
apple
banana
```
- 
| 리스트 사용 | 의미 |
|------------|------|
| ${city[i]} | 리스트 변수 city의 i번째 원소 |
| ${city[*]} | 리스트 변수 city의 모든 원소 |
| ${city[@]} | 리스트 변수 city의 모든 원소 |
| ${#city[*]} | 리스트 변수 city내의 원소 개수 |
| ${#city[@]} | 리스트 변수 city내의 원소 개수 |

### 읽기 전용 변수(readonly) - 값 고정
```
$ vi readonly.sh
$ #!/bin/bash
$ readonly pi=3.14
$ echo $pi
$ chmod +x readonly.sh
$ ./readonly.sh
3.14
```

### 환경 변수(export)
- export 변수명=값
```
$ vi export.sh
$ #!/bin/bash
$ export MYVAR="hello"
$ echo $MYVAR
hello
```

### 입력 변수(read)
```
$ vi read.sh
$ #!/bin/bash
$ read name
$ echo $name
$ chmod +x read.sh
$ ./read.sh
park
park
```
```
$ vi read2.sh
$ #!/bin/bash
$ read x y z
$ echo $x
$ echo $y
$ echo $z
$ chmod +x read2.sh
$ ./read2.sh
AA BB CC
AA
BB
CC
```

### 명령어 결과 저장
- 변수=$(명령어)
```
$ vi save.sh
$ #!/bin/bash
$ today=$(date)
$ echo $today
$ ./read.sh
2026.04.12~~~~~~~~
park
park
```

### 지역 변수(local)
- 함수 안에서만 사용
```
vi local.sh
#!/bin.bash
func() {
local x=10 // 함수안에서만 사용
echo $x
}
func
10
```

#### 타입처럼 사용하는 정리
- 
| 타입 느낌 | 방법 |
|-----------|------|
| 문자열 | 기본 |
| 숫자 | $(()) |
| 배열 | arr=() |
| 상수 | readonly |
| 환경 변수 | export |
| 지역 변수 | local |

#### 실습
- 문자열 : name="kim"
```
vi str2.sh
#!/bin/bash
name="kim"
echo $name
chmod +x str2.sh
./str2.sh

- kim
```
- 숫자 : a=10, b=20
```
vi ari2.sh
#!/bin/bash
a=10
b=20
echo $a $b
chmod +x ari2.sh
./ari2.sh

- 10 20
```
- 배열 : (apple banana cherry)
```
vi array2.sh
#!/bin/bash
arr=(apple banana cherry)
echo ${arr[0]}
echo ${arr[1]}
echo ${arr[2]}

- apple
- banana
- cherry
```
- 환경 변수 : MYVAR="hello"
```
vi export.sh
#!/bin/bash
export MYVAR="hello"
echo $MYVAR

- hello
```
- 함수
```
vi func2.sh
#!/bin/bash
read a b
func() {
local sum=$((a + b))
echo "합: $sum"
}
func

- 7 3
- 합: 10
```

### 표준 입력 읽기
- read 명령어
  - 표준입력에서 한 줄을 읽어서 단어들을 변수들에 순서대로 저장
  - 남은 단어들은 마지막 변수에 모두 저장
```
$ read 변수1 ..... 변수 n
$ read x y
hello world
$ echo $x
hello
$ echo $y
world
```
- 변수를 하나만 사용
```
$ read x
hello
$ read $x
hello
```

### 연산(Arithmetic)
```
a=5
b=3

sum=$((a + b))
echo $sum
```
- $ : 명령어 실행
- $(()) : 숫자 연산

#### 연산(Arithmetic) 실습
```
vi ari3.sh
#!/bin/bash

read a b

echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))

5 2
7
3
10
2
```

### 조건문
```
if[조건]
then
 실행문
else
 실행문
fi
```
```
vi if.sh
#!/bin/bash
read num
if [ $num -gt 10 ]
then
   echo "10보다 큼"
else
   echo "10이하"
fi

12
10보다 큼
8
10이하
```
```
vi if2.sh
#!/bin/bash
read score
if [ $score -ge 60 ]
then
    echo "합격"
else
    echo "불합격"
fi

50
불합격
80
합격
```

#### 조건문 응용 문제
```
vi iftest.txt
#!/bin/bash
read -p "input(start/stop/restart): " cmd

if [ "$cmd" == "start" ]
then
    echo "start"
elif [ "$cmd" == "stop" ]
then
    echo "stop"
elif [ "$cmd" == "restart" ]
then
    echo "restart"
else
    echo "input error"
fi

input(start/stop/restart): dwewe
input error
input(start/stop/restart): start
start
```

### 비교 연산자
- 
| 의미 | 연산자 |
|------|--------|
| 같다 | -eq(==) |
| 크다 | -gt(>) |
| 작다 | -lt(<) |
| 같지 않다 | -ne(!=) |
| 크거나 같다 | -ge(>=) |
| 작거나 같다 | -le(<=) |

### 반복문
```
for 변수 in 값들
do
   실행문
done
```
```
for i in 1 2 3
do
  echo $i
done
```
```
i = 1
while [$i -le 5]
do
  echo $i
i = $((i+1))
done
```

#### 반복문 실습
```
$ vi for.sh

#!/bin/bash

echo "이름 입력:"
read name

echo "숫자 입력:"
read num

if [ $num -gt 10 ]
then
  echo "$name님, 숫자가 큽니다"
else
  echo "$name님, 숫자가 작습니다"
fi

for in {1..3}
do
  echo "Hello $name"
done

$ chmod +x for.sh
$ ./for.sh
```

### 파일 처리
```
if [ -f file.txt ]
then
  echo "파일 존재"
fi
```
- 
| 옵션 | 의미 |
|------|------|
| -f | 파일 존재 (파일만 검사) |
| -d | 디렉토리 (디렉토리만 검사) |
| -e | 존재 여부 (전부 검사) |

### 함수
```
$ vi func3.sh

#!/bin/bash

hello() {
  echo "Hello Linux"
}

hello
```

### 함수 + 변수
```
$ vi func4.sh

#!/bin/bash

print_name() {
  echo "My name is $name"
}

name="Kim"
print_name
```

### 함수 + 입력값
```
$ vi func5.sh

#!/bin/bash

sum() {
  result=$((a+b))
  echo "합:$result"
}

a=10
b=20

sum
```

### 함수 + 파라미터
```
함수이름 값1, 값2

$1 → 첫 번째 값
$2 → 두 번째 값
```
```
$ func6.sh

#!/bin/bash

add() {
  echo "합:$(($1+$2))"
}
add 10 20
```

#### 예제 1 : 계산기
```
$ vi calc.sh

#!/bin/bash

read a
read b

sum=$((a+b))
echo "합계:$sum"
```

#### 예제 2 : 짝수 출력
```
$ vi even.sh

#!/bin/bash

for i in {1..10}
do
if [ $((i%2)) -eq 0 ]
then
  echo $i
fi
done
```

#### 예제 3 : 파일 존재 확인
```
$ vi checkfile.sh

#!/bin/bash

read filename

if [ -f $filename ]
then
  echo "파일 존재"
else
  echo "파일 없음"
fi
```

#### 간단한 ATM 프로그램
```
[요구사항]

초기 잔액 : 1000
deposit / withdraw 입력
 [입급]    [출금]
금액 입력
잔액 계산
```
```
$ vi atm.sh

#!/bin/bash

# 초기 잔액
balance=1000 # 초기값 설정, 변수선언(공백없이)

echo "현재 잔액:$balance"

# 작업 선택
echo "deposit 또는 withdraw 입력하세요:"
read action # 입력값을 변수에 저장

# 금액 입력
echo "금액을 입력하세요:"
read amount # 입력값을 변수에 저장

# 조건 처리
if [ "$action" = "deposit" ]
then
  balance=$((balance + amount)) echo "입금 완료"
  echo "현재 잔액:$balance"
elif [ "$action" = "withraw" ]
then
  if [$amount -gt $balance ]
  then
    echo "잔액 부족"
  else
    balance=$((balance - amount))
    echo "출금 완료"
    echo "현재 잔액:$balance"
  fi
  else
    echo "잘못된 입력입니다"
  fi
```
```
[실행 예시]

현재 잔액: 1000
deposit 또는 withdraw 입력하세요:
deposit
금액을 입력하세요:
500

입금완료
현재 잔액: 1500
```

### Shell Script 저장 방법
```
atm.sh

atm → 프로그램 이름
.sh → shell script 파일이라는 의미

touch atm.sh
vi atm.sh

chmod +x atm.sh
./atm.sh
```

### 실습 문제(1 ~ 19)
- 1. hello.sh 파일을 생성하고 "Hello Linux"를 출력하는 스크립트를 작성하시오
```
$ vi hello.sh

#!/bin/bash

echo "Hello Linux"
```
- 2. 현재 날짜와 시간을 출력하는 스크립트를 작성하시오
```
#!/bin/bash

date
```
- 3. 현재 사용자 이름을 출력하는 스크립트를 작성하시오
```
#!/bin/bash

whoami
```
- 4. 현재 작업 디렉토리를 출력하는 스크립트를 작성하시오
```
#!/bin/bash

pwd
```
- 5. 변수 name에 본인 이름을 저장하고 출력하시오
```
#!/bin/bash

name="박지안"
echo $name
```
- 6. 두 숫자를 변수에 저장하고 합을 출력하시오
```
#!/bin/bash

a=10
b=20
sum=$((a+b))
echo $sum
```
- 7. 사용자로부터 이름을 입력받아 출력하는 스크립트를 작성하시오
```
#!/bin/bash

echo "이름을 입력하시오:"
read name
echo $name
```
- 8. 사용자로부터 숫자를 입력받아 2배 값을 출력하시오
```
#!/bin/bash

echo "숫자를 입력하시오:"
read num
echo $((num * 2))
```
- 9. 현재 날짜를 변수에 저장하여 출력하시오
```
#!/bin/bash

today=$(date)
echo $today
```
- 10. 입력받아 숫자가 10보다 크면 "크다"를 출력하시오 -gt : greater than
```
#!/bin/bash

read num

if [ $num -gt 10 ]
then
  echo "크다"
fi
```
- 11. 입력받은 숫자가 짝수인지 홀수인지 출력하시오
```
#!/bin/bash

read num

if [ $((num % 2)) -eq 0 ]
then
  echo "짝수"
else
  echo "홀수"
fi
```
- 12. 입력받은 점수가 60 이상이면 "합격", 아니면 "불합격" 출력 -ge : greater or equal
```
#!/bin/bash

read score

if [ $score -ge 60 ]
then
  echo "합격"
else
  echo "불합격"
fi
```
- 13. 파일이 존재하는지 확인하는 스크립트를 작성하시오
```
#!/bin/bash

if [ -f file.txt ]
then
  echo "파일 존재"
else
  echo "없음"
fi
```
- 14. 사용자가 root인지 확인하여 메시지 출력
```
#!/bin/bash

if [ $USER = "root" ]
then
  echo "관리자"
else
  echo "일반 사용자'
fi
```
- 15. 1부터 5까지 숫자를 출력하시오 {1..5} : 범위 반복
```
#!/bin/bash

for i in {1..5}
do
  echo $i
done
```
- 16. 1부터 10까지 합을 구하는 스크립트를 작성하시오
```
#!/bin/bash

sum=0

for i in {1..10}
do
  sum=$((sum + i))
done
echo $sum
```
- 17. 현재 디렉토리의 파일 목록을 반복문으로 출력하시오
```
#!/bin/bash

for file in *
do
  echo $file
done
```
- 18. 사용자가 입력한 숫자까지 반복 출력
```
#!/bin/bash

read num

for ((i=1; i<=num; i++))
do
  echo $i
done
```
- 19. 무한 반복으로 "Hello"를 출력하다가 종료하는 스크립트 작성
```
#!/bin/bash

while true
do
  echo "Hello"
  sleep 1
done
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-04
