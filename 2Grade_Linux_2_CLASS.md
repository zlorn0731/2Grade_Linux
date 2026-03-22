# 👨‍💻리눅스와 유닉스

## 3장 파일 권한 관리

### 학습 목표
- 리눅스 파일 권한 구조 이해
- 파일 링크
- 파일 권한 확인 방법
- chmod
- chown
- 디렉토리 권한의 의미
- 실제 권한 설정 실습

### 권한 개념
- 리눅스는 다중 사용자 운영체제이므로 파일 접근 권한을 관리해야 함
- 
| 사용자 | 의미 |
|--------|-----------|
| owner | 파일 소유자 |
| group | 파일이 속한 그룹 |
| others | 기타 사용자 | 

### 파일 권한 종류
-
| 권한 | 의미 | 기호 |
|------|-----|------|
| Read | 읽기 | r |
| Write | 쓰기 | w |
| Execute | 실행 | x |

### 파일 권한 확인
- -rwxr-xr-- l student student 1024 Mar 10 file.sh
- -[rwxr-xr--]
  - rwx : 소유자
  - r-x : 그룹
  - r-- : 기타
- [-]rwxr-xr-- : 파일 종류 표시
  - '-' : 일반 파일
  - d : 디렉토리
  - l : 링크

### 링크
- 하나의 파일을 여러 이름으로 접근하는 방법
-
| 종류 | 명령어 | 특징 |
|------|-------|-------|
| 하드 링크 | ln | 원본과 동일한 파일 |
| 심볼 링크 | ln -s | 원본을 가리키는 바로가기 | → 주로 사용 ⭐
- 하드 링크 : 같은 파일을 다른 이름으로 하나 더 만든 것
  - 원본과 완전히 동일한 파일
  - inode 공유
  - 원본 삭제해도 데이터 유지
```
touch file.txt
ln file.txt file_link.txt → ln : 같은 주소 인지 inode로 확인 함
```
- 심볼릭 링크 : 원본 파일을 가리키는 바로가기 파일
  - 원본 파일을 참조
  - 원본 삭제하면 링크 깨짐 ⭐
  - 다른 파일 시스템도 가능
```
ln -s 원본파일 링크 파일
ln -s file.txt file_symlink.txt
```

### 하드 링크 실습
```
touch original.txt
echo "Hello Linux" > original.txt
ln original.txt hardlink.txt

ls -li original.txt hardlink.txt → inode 번호 확인 (같으면 하드 링크)

cat original.txt hardlink.txt → 내용 확인 (동일 내용 출력됨)

rm original.txt → 원본 삭제 / hardlink.txt는 원본 갖고 있음
```

### 심볼릭 링크 실습
```
touch original.txt
echo "Symbolic Link Test" > original.txt
ln -s original.txt symlink.txt

ls -li original.txt symlink.txt → inode 번호 확인 (다르면 하드 링크 아님)

cat symlink.txt → 내용 확인 (동일 내용 출력됨)

rm original.txt → 원본 삭제

cat symlink.txt → 에러 발생 (링크 깨짐)
```

### 왜 링크를 사용하는가?
- 저장 공간 절약
  - 하드 링크는 복사 없이 공유
- 경로 단축
  - 긴 경로 대신 사용 ⭐
- 데이터 보호
  - 하드 링크는 원본 삭제에도 유지
-
| 항목 | 하드 링크 | 심볼릭 링크 |
|------|----------|-------------|
| 원리 | 동일 파일 | 경로 참조 |
| inode | 동일 | 다름 |
| 원본 삭제 | 유지 | 깨짐 |
| 디렉토리 | 불가 | 가능 |

### cp와 Hard link의 차이점
-
| 구분 | cp | 하드링크 (ln) |
|------|-----|--------------|
| 파일 개수 | 2개(서로 다른 파일) | 2개 이름, 실제는 1개 |
| inode | 다름 | 동일 |
| 데이터 | 각각 따로 존재 | 동일 데이터 공유 |
| 수정 | 서로 영향 없음 | 같이 변경됨 |
| 저장 공간 | 2배 사용 | 추가 사용 거의 없음 |

### 숫자를 이용한 파일 접근 권한 변경
- 소유자, 그룹, 기타 사용자의 권한을 숫자로 환산하여 전체 접근 권한을 표기 가능
-
| 소유자 권한 | 그룹 권한 | 기타 사용자 권한 |
| [r][w][-] | [r][-][-] | [r][-][-] |
| [4][2][1] | [4][2][1] | [4][2][1] |
| 십진수 환산(6) | 십진수 환산(4) | 십진수 환산(4) |
-
| 접근 권한 | 숫자 모드 | 접근 권한 | 숫자 모드 |
|----------|-----------|----------|------------|
| rwxrwxrwx | 777 | rw-r--r-- | 644 |
| rwxr-xr-x | 755 | rwx------ | 700 |
| rw-rw-rw- | 666 | rw-r----- | 640 |
| r-xr-xr-x | 555 | r-------- | 400 |
- r : 4, w : 2, x : 1
### chmod 명령어
- chmod | 숫자(0~7) | 숫자(0~7) | 숫자(0~7 |파일명
```
chmod 권한 파일명
chmod 755 file.sh → rwxr-xr-x
```
#### (예시)
```
777
- chmod 777 file.txt
- 777 → rwxrwxrwx
- 모든 사용자 읽기/쓰기/실행 가능

755
- chmod 755 file.txt
- 755 → rwxr-xr-x
- 소유자만 수정 가능

 644
 - chmod 644 file.txt
 - 644 → rw-r--r--
 - 소유자만 쓰기 가능
 - 일반 문서 파일에서 많이 사용
```
### 문자를 이용한 파일 접근 권한 변경
-
| 구분 | 문자/기호 | 의미 |
|------|----------|-------|
| 사용자 카테고리 문자 | u | owner : 파일 소유자 |
| 사용자 카테고리 문자 | g | group : 파일 소유 그룹 |
| 사용자 카테고리 문자 | o | others : 소유자와 그룹 이외의 기타 사용자 |
| 사용자 카테고리 문자 | a | All : 전체 사용자 |
| 연산자 기호 | + | 권한 부여 |
| 연산자 기호 | - | 권한 제거 |
| 연산자 기호 | = | 접근 권한 설정 |
| 접근 권한 문자 | r | 읽기 권한 |
| 접근 권한 문자 | w | 쓰기 권한 | 
| 접근 권한 문자 | x | 실행 권한 |
#### (예시)
```
+x
- chmod +x file.sh 
- 실행 권한 추가
  
u+w
- chmod u+w file.txt
- 소유자에게 쓰기 권한 추가
  
o-x
- chmod o-x file.sh 
- 기타 사용자 실행 권한 제거
  
go+w
- chmod go+w file.txt
- 그룹과 기타 사용자에게 쓰기 권한 추가
```

### chown 명령어 (Change Owner)
- chown : 파일의 소유자(Owner)를 변경하는 명령어
```
ls -l file.txt
-rw-r--r-- park students 100 Mar 10 file.txt → park : 소유자 | students : 그룹
chown kim file.txt → 소유자 변경
kim → 소유자
```

### chgrp 명령어 (Change Group)
- chgrp : 파일의 그룹(Group)을 변경하는 명령어
```
ls -l file.txt
-rw-r--r-- park students 100 Mar 10 file.txt → park : 소유자 | students : 그룹
chgrp developers file.txt → 그룹 변경
developers → 그룹
```

### 디렉토리 권한
- 디렉토리 권한 의미는 파일과 약간 다름
-
| 권한 | 의미 |
|------|------|
| r | 디렉토리 목록 보기 |
| w | 파일 생성/삭제 |
| x | 디렉토리 접근 |
#### (예시)
```
drwxr-xr-x
의미
소유자 → 모든 작업 가능
그룹 → 읽기 + 실행
기타 → 읽기 + 실행
```

### 디렉토리 권한 재귀 변경
```
기본 문법 : chmod -R 755 directory
-R
- chmod -R 755 project
- 하위 디렉토리까지 변경
```

### 기본 접근 권한 설정
- 파일이나 디렉토리를 생성할 때 기본 접근 권한이 자동 설정됨
  - 일반 파일 : 소유자와 그룹 → 읽기(r), 쓰기(w) / 기타 사용자 → 읽기(r)만 가능
  - 디렉토리 : 소유자와 그룹 → 읽기(r), 쓰기(w), 실행(x) / 기타 사용자 → 읽기(r), 실행(x) 가능
- 계정 정보 보는 법
  - cd /etc → etc : 시스템 설정 파일들이 모여 있는 디렉토리
  - cat passwd → 계정 정보 ~~~~ 뜸
- 마스크 값의 의미
  - 파일이나 디렉토리를 생성할 때 부여되지 않을 권한을 지정하는 값
  - 현재 마스크 값이 002라면, -------w-가 됨
  - umask -S 옵션을 사용하면 문자 모드로 출력 가능
```
umask
0002
umask -S
u=rwx, g=rwx, o=rx
```
  - 마스크 값 변경하기
  - 마스크 값을 변경하면 기본 접근 권한이 달라짐
```
umask 077
umask
0007
umask -S
u=rwx, g=, o=
touch file.txt
ls -l file.txt
-rwx------ ~~~~~~~~ file.txt
```

### 실습
```
파일 생성 및 확인
touch test.txt
ls -l test.txt

권한 변경 및 확인
chmod +x test.txt
ls -l test.txt

권한 숫자 설정 및 확인
chmod 755 test.txt
ls -l test.txt

디렉토리 생성
mkdir project
권한 변경 및 확인
chmod 700 project
ls -ld project

소유자 변경 및 확인
sudo chown root test.txt
ls -l test.txt
```

#### 연습 문제
```
[문제 1]
다음 권한의 의미를 설명하시오
-rwxr-xr--
> 기본 파일 → 소유자 : 읽기, 쓰기, 실행 가능 | 그룹 : 읽기, 실행만 가능 | 기타 사용자 : 읽기만 가능

[문제 2]
다음 권한을 설정하는 명령어 작성
rwxr-xr-x
> 755

[문제 3]
다음 작업 수행
linux 디렉토리 생성
file.txt 생성
권한을 644로 설정
실행 권한 추가
> mkdir linux
> touch file.txt
> chmod 644 file.txt
> chmod +x file.txt
```

### 실습 (1~11문제)
```
[문제 1]
현재 작업 디렉토리를 확인하시오.
> pwd

[문제 2]
home 디렉토리로 이동하시오.
> cd ~

[문제 3]
다음 디렉토리를 생성하시오.
linux
> mkdir linux

[문제 4]
linux 디렉토리 안에 다음 디렉토리를 생성하시오.
linux
   └ study
   └ practice
> cd linux
> mkdir study practice

[문제 5]
study 디렉토리로 이동하시오.
> cd study

[문제 6]
다음 파일을 생성하시오.
file1.txt
file2.txt
file3.txt
> touch file1.txt file2.txt file3.txt

[문제 7]
현재 디렉토리의 파일 목록을 상세 정보와 함께 출력하시오.
> ls -lrt

[문제 8]
숨김 파일까지 포함하여 파일 목록을 출력하시오.
> ls -lart

[문제 9]
file1.txt 파일을 복사하여 file1_backup.txt파일을 생성하시오.
> cp file1.txt file1_backup.txt

[문제 10]
file2.txt파일을 practice 디렉토리로 이동하시오
> mv file2.txt /aaa/linux/practice

[문제 11] 
practice 디렉토리를 삭제하시오.
> rm -r practice
```

### 권한 실습 (1~10문제)
```
[문제 1]
다음 파일을 생성하시오
script.sh
> touch script.sh

[문제 2]
script.sh파일에 실행 권한을 추가하시오.
> chmod +x script.sh

[문제 3]
script.sh파일 권한을 다음과 같이 변경하시오.
rwxr-xr-x
> chmod 755 script.sh

[문제 4]
test.txt파일 권한을 다음과 같이 변경하시오.
rw-r--r--
> chmod 644 test.txt

[문제 5]
다음 디렉토리를 생성하시오.
secure
권한을 700으로 설정하시오.
> mkdir secure
> chmod 700 secure

[문제 6]
secure디렉토리 권한을 다음과 같이 변경하시오.
rwxr-x---
> chmod 750 secure

[문제 7]
script.sh파일에서 others의 실행 권한을 제거하시오.
> chmod o-x script.sh

[문제 8]
test.txt파일에 gruop의 쓰기 권한을 추가하시오.
> chmod  g+w test.txt

[문제 9]
project디렉토리와 모든 하위 파일의 권한을 755로 변경하시오.
> chmod -R 755 project

[문제 10]
test.txt파일의 소유자를 root로 변경하시오.
> sudo chown root test.txt
```

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-03-22
