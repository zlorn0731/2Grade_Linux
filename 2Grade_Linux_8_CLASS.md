# 👨‍💻리눅스와 유닉스

## 9장 리눅스 프로그래밍 기초

### 프로그래밍이란?
- 컴파일과 인터프리터
  - 컴파일 : 고급 언어로 작성된 프로그램의 전체 코드를 한 번에 번역하여 기계어로 된 실행 파일을 생성
  - 인터프리터 : 실행 파일을 미리 생성하지 않고, 프로그램 실행 시 코드를 하나씩 번역하여 실행
  - 일반적으로 인터프리터 방식은 문법이 더 유연하고 배우기 쉬움
  - 셸 스크립트는 인터프리터 방식으로 실행

### C코딩과 컴파일하기
- C 컴파일러 설치
  - 고급 언어를 기계어로 변환하는 소프트웨어는 컴파일러
  - C 언어 프로그램을 컴파일하려면 C 컴파일러가 필요
  - 리눅스에서 사용하는 C 컴파일러는 GNU C 컴파일러(gcc)
  ```
  $ gcc
   명령어 'gcc'을(를) 찾을 수 없습니다. 그러나 다음을 통해 설치할 수 있습니다:
  sudo apt install gcc
  ```
- gcc 설치 완료
```
$ gcc
gcc: fatal error: no input files
compilation terminated.
```

- 간단한 C 프로그램 컴파일 및 실행
  - gcc
    - 형식 : gcc [옵션] C 파일
    - 옵션
      - -c : 오브젝트 파일을 생성(.o 파일)
      - -l : 라이브러리를 지정
      - -o 파일명 : 지정한 파일명으로 실행 파일을 생성
    - 사용 예
      - gcc test.c
      - gcc -o test test.c
      - gcc -lstdc++ test.cpp

- C 프로그램 컴파일하기
  - study 디렉토리 내에 ch12 디렉터리를 생성하고 해당 디렉터리에서 실습 실행
```
[예제 12-1 hello.c]

#include <stdio.h>
int main()
{
  printf("Hello, World\n");
}
```
  - gcc 소스파일명 명령으로 C 프로그램을 컴파일
  - 소스 코드에 오류가 없으면 실행 파일이 a.out이라는 이름으로 생성
```
$ gcc hello.c
$ls
a.out hello.c
```

- C 프로그램 실행하기
  - a.out을 실행하려면 경로를 지정해야 함
  ```
  $ a.out
  a.out : 명령을 찾을 수 없습니다
  ```
  - 현재 디렉터리를 지정하여 실행
  ```
  $ ./a.out
  Hello, World
  ```

- 실행 파일명 변경하기
  - gcc로 생성한 기본 실행 파일은 a.out
  - -o 옵션을 사용하여 실행 파일 이름을 지정할 수 있음
  ```
  $ gcc -o hello hello.c
  $ ./hello
  Hello, World
  ```

- 다중 소스 파일 컴파일하기
  - 하나의 소스 파일이 아닌 여러 개의 소스 파일로 프로그램을 작성하는 경우가 많음
  - 각 소스 파일을 개별적으로 컴파일하여 오브젝트 파일을 생성하고, 이를 링크하여 실행 파일을 만듦

### C코딩 실습1
- C 언어에서 함수의 사용법을 간단히 다룬 예시로 main.c 파일을 작성함
```
#include <stdio.h>

extern int add(int a, int b);
extern int sub(int a, int b);

int main() {
  int num1, num2, res;
  num1 = 10;
  num2 = 20;

  res = add(num1, num2);
  printf("%d + %d = %d\n", num1, num2, res);
  res = sub(num1, num2);
  printf("%d - %d = %d\n", num1, num2, res);
}
```
- main.c를 그냥 컴파일하면 오류 메시지가 출력
- main.c에서 add와 sub 함수가 정의되지 않아 참조 오류가 발생
- 이를 해결하려면 add와 sub 함수를 별도의 파일로 작성하고, 이 파일들을 함께 컴파일해야 함

- add.c 코딩
```
#include <stdio.h>

int add(int n1, int n2)
{
  int sum;

  sum = n1 + n2;
  return sum;
}
```
- sub.c 코딩
```
#include <stdio.h>

int sub(int n1, int n2)
{
  int num;

  num = n1 - n2;
  return num;
}
```
- add.c를 컴파일하면 main함수가 없다는 오류가 발생
- add.c는 함수 정의만 포함하고 있기 때문에 독립적으로 실행할 수 없음

- 실행 파일 생성 시 각 소스 파일을 개별적으로 컴파일하여 오브젝트 파일 생성 필요
- 오브젝트 파일 생성은 gcc에서 -c 옵션을 지정하면 됨
- gcc -c main.c 실행 시 main.o파일 생성됨
- .o 파일은 바이너리 형태의 오브젝트 파일로, vi나 cat으로 내용 확인 불가능
```
$ ls
add.c  hello  hello.c  main.c  sub.c
$ gcc -c main.c
$ gcc -c add.c
$ gcc -c sub.c
$ ls
add.c add.o hello hello.c main.c main.o sub.c sub.o
```
- 생성된 오브젝트 파일을 연결하여 실행 파일 생성
```
$ gcc -o multi main.o add.o sub.o
$ ./multi
10 + 20 = 30
10 - 20 = -10
```

### 시스템호출과 라이브러리 함수
- 리눅스는 C언어로 구현
- 일반적인 C 구문과 함수를 사용할 수 있으며, 리눅스 시스템 호출(sytem call)도 사용 가능
- 시스템 호출 : 리눅스의 기능을 직접 이용하는 함수 → 시스템 프로그래밍에 활용
- 라이브러리(library) 함수 : 미리 컴파일된 함수들의 집합
  - 데이터 입출력, 수학 연산, 문자열 처리 등 다양한 기능 제공
- 시스템 호출과 라이브러리는 유사하지만 차이점이 있음
- 프로그래밍 시 상황에 맞게 시스템 호출과 라이브러리를 적절히 선택해야 함

- 시스템 호출
  - 시스템 호출은 C 함수와 유사한 형식으로 사용됨
  - 미리 정의된 이름을 사용하여, 인자는 호출 종류에 따라 다를 수 있음
  ```
  리턴값 = 시스템 호출명(인자, ...);
  ```
- 라이브러리 함수
  - 자주 사용하는 기능을 묶어 제공하는 함수
  - 목적 : 코드 재사용으로 개발 및 디버깅 용이, 컴파일 속도 향상
  - 리눅스에서 라이브러리 위치
    - 일반적으로 /usr/lib
    - /lib는 /usr/lib의 심볼릭 링크

- 시스템 호출과 라이브러리 함수의 비교
  - 시스템 호출
    - 커널의 해당 모듈을 직접 호출하여 작업 수행
    - 커널의 기능을 직접 사용하므로 시스템 호출이라 불림
  - 라이브러리 함수
    - 커널을 직접 호출하지 않고 추가 기능을 제공
    - 내부적으로 시스템 호출을 사용할 수도 있음
    - 프로그래머는 내부적으로 시스템 호출이 사용되는지 신경 쓸 필요 없음
  - 응용 프로그램은 라이브러리 함수와 시스템 호출을 모두 활용 가능

### make 사용하기
- 다중 소스 파일을 매법 gcc로 컴파일하는 것은 번거로움
- 이를 자동화하기 위해 make 명령 사용
- make는 Makefile(또는 makefile)을 읽어 컴파일 및 링크 수행

- make 명령 설치하기
  - make 명령이 설치되어 있는지 확인
  - 설치되지 않은 경우, 다음과 같은 메시지가 표시
  ```
  $ make
  명령어 'make'을(를) 찾을 수 없습니다. 그러나 다음을 통해 설치할 수 없습니다:
  sudo apt install make
  sudo apt install make-guile
  ```
- sudo apt 명령으로 make 명령을 설치
```
$ sudo apt install make
```
- 소스 파일 준비
  - make 명령을 사용하기 위해 2절에서 만든 main.c add.c sub.c 파일 활용

- make file 작성하기
```
TARGET=mymath
OBJS=main.o add.o sub.o

$(TARGET): $(OBJS)
  gcc -o $(TARGET) $(OBJS)

main.o: main.c
add.o: add.c
sub.o: sub.c
```
  - 1행 ~ 2행 : TARGET이나 OBJS는 사용자가 임의로 정의한 변수
  - 4행 : TARGET은 OBJS로 생성한다고 설정
  - 5행 : TARGET을 생성하는 실제 명령
  - 7행 ~ 9행 : 오브젝트 파일 main.o, add.o, sub.o가 어느 파일에서 생성되는지 명시
```
#clean

clea:
rm -f $(OBJS) $(TARGET)
```

- main.o, add.o, sub.o 파일을 먼저 삭제
```
$ rm *.o
rm: 일반 파일 'add.o'를 제거할까요? y
rm: 일반 파일 'main.o'를 제거할까요? y
rm: 일반 파일 'sub.o'를 제거할까요? y
$ make
cc    -c -o main.o main.c
cc    -c -o add.o add.c
cc    -c -o sub.o sub.c
gcc -o mymath main.o add.o sub.o
```
- make 실행 후 생성된 실행 파일 mymath를 실행
```
$ ./mymath
10 + 20 = 30
10 - 20 = -10
```

- make는 변경된 파일만 다시 컴파일하여 실행 파일을 생성
- 전체 소스를 다시 컴파일할 필요 없이 효율적으로 빌드 가능
```
#include <stdio.h>

int add(int n1, int n2)
{
  int sum;

  printf("add.c\n");    → 추가한 내용
  sum = n1 + n2;
  return sum;
}
```
- 다시 make 명령을 실행하면 변경된 add.c만 다시 컴파일하여 실행 파일을 생성
```
$ make
cc    -c -o add.c add.c
gcc -o mymath main.o add.o sub.o
$ ./mymath
add.c
10 + 20 = 30
10 - 20 = -10
```

### make 사용하기 (실습하기)
- 1. 다음 C프로그램을 코딩한다(파일명 : ch12-1.c)
```
[han.c]

#include <stdio.h>

extern int bit();

int main()
{
  printf("HAN");
  bit();
  printf("Academy\n");
}
----------------------------
[bit.c]

#include <stdio.h>

int bit();
{
  printf("BIT\n");
}
```
- 2. han.c와 bit.c를 컴파일하여 실행 파일 hanbit을 생성하는 makefile을 작성
- 3. make 명령을 사용하여 소스를 컴파일하고 실행 파일을 생성
- 4. 생성된 hanbit을 실행하고 출력을 확인

### 파이썬 코딩하기
- 파이썬 환경 구축하기
  - 파이썬 설치 확인
  ```
  $ python3
  ```
  - 파이썬 환경을 종료하려면 exit()나 Ctrl + D를 입력
  ```
  exit()
  ```
- 파이썬 코딩하기
  - 파이썬은 >>> 프롬프트에서 직접 코드 입력 가능
  - 스크립트 언어로 컴파일 과정 없이 바로 실행됨
  ```
  $ python3
  ```
- 파이썬 스크립트 파일 작성
```
$ vi hello.py
  # hello.py
  print("Hello Python!")
  han=10
  bit=10
  print(han+bit)
:wq
```
- 파일로 작성된 파이썬 스크립트는 python3 파일명 명령으로 실행
```
$ python3 hello.py
Hello Python!
20
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-09
