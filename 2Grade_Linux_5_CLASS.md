# 👨‍💻리눅스와 유닉스

## 5장 셸(shell) 사용법

### 쉘
- 쉘은 사용자와 운영체제 사이에 창구 역할을 하는 소프트웨어
- 명령어 처리기(command processor)
- 사용자로부터 명령어를 입력받아 이를 처리함
```
명령어 → 쉘 → 명령어 실행
```
- 
| 쉘의 종류 | 쉘 실행 파일 |
|-----------|-------------|
| 본 쉘 | /bin/sh |
| 콘 쉘 | /bon/ksh |
| C 쉘 | /bin/csh |
| Bash | /bin/bash |
| tcsh | /bin/tcsh |

### 시작 파일 (start-up file)
- 본 쉘
```
/etc/profile
~/.profile
```
- bash
```
/etc/profile
/etc/bashrc
~/.bash_proflie
~/.bashrc → 조심히 다뤄야 해서 숨김
```
- C 쉘
```
/etc/.login
~/.login
~/.cshrc
```

### 쉘 기본 사용법

#### 특수문자 *
- 
| 사용 예 | 의미 |
|---------|------|
| ls * | 현재 디렉토리의 모든 파일과 서브 디렉토리를 출력 |
| ls *.txt | 현재 디렉토리에서 .txt로 끝나는 모든 파일을 출력 |
| ls a* | 현재 디렉토리에서 a로 시작하는 모든 파일과 서브 디렉토리를 출력 |
| ls a*t | 현재 디렉토리에서 a로 시작하고 t로 끝나는 모든 파일과 서브 디렉토리를 출력 |

#### 특수문자 ?와[]
- 
| 사용 예 | 의미 |
|---------|------|
| ls ?.txt | .txt 앞에 한 글자를 가진 파일을 출력 |
| ls hello.? | hello. 다음에 한 글자를 가진 파일을 출력 |
| ls data[123] | data 다음에 1, 2, 3 중 하나가 오는 파일을 출력 |
| ls [1-9]*.txt | 1에서 9 사이의 숫자로 시작하는 임의 이름을 가진 .txt 파일을 출력 |

#### 특수문자 ~와-
- 
| 사용 예 | 의미 |
|---------|------|
| cd ~ | 현재 사용자 계정의 홈 디렉토리로 이동 |
| cd ~user2 | user2 계정의 홈 디렉토리로 이동 |
| cp *.txt ~/tmp | 확장자가 txt인 모든 파일을 현재 사용자 계정의 홈 디렉토리 아래에 있는 tmp 디렉토리로 복사 |
| cp ~user2/linux.txt | user2 계정의 홈 디렉토리 아래에서 linux.txt 파일을 찾아 현재 디렉토리로 복사 |
| cd - | 이전 작업 디렉토리로 이동 |

#### 특수문자 ;과|
- 
| 사용 예 | 의미 |
|---------|------|
| pwd;ls | 왼쪽부터 차례대로 pwd 명령을 실행한 후 ls 명령을 실행 |
| cat /etc/services\|more | /etc/services 파일의 내용을 한 화면씩 출력 |


#### 특수문자 '', "", ``
- 
| 사용 예 | 의미 |
|---------|------|
| echo '$SHELL' | 쉘 변수를 의미하는 $의 기능이 없어지고 그냥 $SHELL 문자열을 출력 |
| echo "$SHELL" | 환경 변수인 SHELL에 저장된 값이 출력 |
| echo "Today:`date`" | `date`가 명령으로 해석되어 현재 날씨와 시간을 출력하고, 그 결과를 포함한 문자열을 출력 |

#### 복합 명령어
```
명령어 열(command sequence)
$ 명령어1; .........; 명령어n
- $date; who; pwd

명령어 그룹(command group)
$ (명령어1; ........; 명령어n)
- $ date; who; pwd > out1.txt → () ❌ = 맨 마지막만 나옴
- $ (date; who; pwd) > out2.txt → 세개 다 나옴
```

#### 환경 변수
- 일반적으로 대문자로 표기
- 사용자 정보, 명령 검색 경로, 프롬프트 설정 등 작업 환경을 관리하는 변수 포함
- 
| 환경 변수 | 의미 |
|-----------|------|
| HISTSZIE | 히스토리 저장 크기 |
| HOME | 사용자 홈 디렉토리의 절대 경로 |
| LANG | 사용하는 언어 |
| LONGNAME | 사용자 계정 이름 |
| PATH | 명령을 탐색할 경로 |
| PWD | 작업 디렉토리의 절대 경로 |
| SHELL | 로그인 쉘 |

#### 전체 변수 출력하기
- 쉘 변수와 환경 변수 모두 출력 : set
- 출력 내용이 많을 경우 | more 옵션을 사용하여 페이지 단위로 확인 가능

#### 특정 변수 출력하기
```
기능 : 화면에 한 줄의 문자열로 출력
형식 : echo [-n] [문자열]
옵션 : -n : 마지막에 줄 바꿈을 하지 않음
사용 예시 : echo
           echo text
           echo -n text

echo 명령을 사용하여 문자열을 출력
$ echo linux
linux
$ echo "Ubuntu Linux"
Ubuntu Linux
```

#### 쉘 변수 설정하기
```
형식 : 변수명=문자열
사용 예시 : TEST = test

$ Linux =ubuntu
$ echo = ubuntu
ubuntu
```

#### 환경 변수 설정하기
- 쉘 변수는 기본적으로 env 명령으로 확인되지 않음
- 환경 변수로 설정하려면 export 명령을 사용해야 함 (export LINUX)
```
set | grep LINUX
env | grep LINUX

기능 : 지정한 쉘 변수를 환경 변수로 바꿈
형식 : export [옵션] [쉘 변수]
옵션 : -n : 환경 변수를 쉘 변수로 변경
사용 예시 : export
           export TEST
           export TEST=test
```
- 이미 설정된 쉘 변수를 환경 변수로 변경
```
$ export LINUX
$ env | grep LINUX
LINUX=ubuntu
```
- 쉘 변수 없이 한 번에 환경 변수로 설정
```
$ export LINUX1=rocky
$ env | grep LINUX
LINUX1=rocky
LINUX=ubuntu
```
- 환경 변수 확인
```
$ export -n LINUX1
$ env | grep LINUX
LINUX=ubuntu
```

#### 변수 해제하기
```
기능 : 지정한 변수의 설정을 해제
형식 : unset [변수]
사용 예시 : unset TEST

설정된 변수 해제 → 아무 값도 출력되지 않음(변수가 해제됨)
```

#### 쉘 기본 사용법 실습
```
1. 쉘 변수에 HOSTNAME이 설정되어 있는지 확인한다
- echo $HOSTNAME
2. 환경 변수에 TERM이 설정되어 있는 확인한다
- echo $TERM
3. 환경 변수 LANG에 설정된 값을 출력한다
- echo $LANG
4. 쉘 변수 HAN의 값을 han으로 설정한다
- HAN=han
5. 환경 변수 BIT의 값을 bit로 설정한다
- BIT=bit
6. 쉘 변수 HAN의 값을 확인한다. 값이 출력되는가?
- echo $HAN
7. 환경 변수 BIT의 값을 확인한다. 값이 출력되는가?
- echo $BIT
8. 변수 HAN과 BIT의 설정을 해제한다
- unset HAN BIT
```

#### 입출력 방향 변경
- 표준 입출력 장치
  - 표준 입력 장치 : 쉘이 정보를 입력받는 장치 (기본값 : 키보드)
  - 표준 출력 장치 : 실행 결과를 내보내는 장치 (기본값 : 화면(모니터))
  - 표준 오류 장치 : 오류 메시지를 출력하는 장치 (기본값 : 화면(모니터))
```
키보드 →→표준 입력→→ 쉘 →→→→  
                      →→→→ 모니터
                      →→→→
```
- 출력 션
  - 출력 션은 명령의 실행 결과를 화면 대신 파일로 저장하는 방법
  - 파일 내용 덮어쓰기 (> 사용)
  - 기존 파일의 내용을 삭제하고 새로운 결과를 저장
  - 사용 형식
    명령 1>파일명(파일 1은 표준 출력)
    명령>파일명(1은 생략 가능)
```
$ date>my_date
$ cat my_date
$ date>my_date
$ cat my_date
```
