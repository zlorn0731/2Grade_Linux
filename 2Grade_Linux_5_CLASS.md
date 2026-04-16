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
