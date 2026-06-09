# 👨‍💻리눅스와 유닉스

## 7장 셸 스크립트(shell script) 응용

### 명령어 사용 vs 스크립트 로직 차이
- mkdir, cp, find = 기능 수행
- for, if, array = "어떻게 처리할지 제어"
- 
| 구분 | 명령어 | 스크립트 |
|------|--------|----------|
| 역할 | 실행 | 제어 |
| 반복 | 없음 | 가능 |
| 조건 | 없음 | 가능 |
| 복잡한 로직 | 어려움 | 가능 |
- 단순 명령어는 "한 번 실행"
```
cp *.txt backup/
```
  - 조건 없음, 제어 없음, 단순 복사
- 스크립트 "조건 + 반복 + 판단"
```
for file in *.txt
do
  if [ -f "$file" ]
  then
    cp "$file" backup/
  fi
done
```
  - 조건 있음, 반복 처리, 더 정교한 작업 가능
- 단순 명령어 사용할 경우
```
mkdir backup
cp *.txt backup/
```
  - 한 번 실행이면 충분
- for / if 등을 사용할 경우
  - 조건이 있을 때
```
for file in *.txt
do
  if grep -q "error" "$file"
  then
    cp "$file" backup/
  fi
done
```
- 
| 개념 | 설명 |
|------|------|
| 명령어 | 도구 |
| 스크립트 | 프로그램 |

### 자동 백업(backup)
- 자동 백업을 위한 shell script 실습
```
#!/bin/bash

mkdir -p backup
cp *.txt backup/

echo "백업 완료"
```
```
#!/bin/bash

today=$(date + %Y%m%d)
backup_dir="backup_$today"

mkdir -p $backup_dir
cp *.txt $backup_dir

echo "백업 완료 : $backup_dir"
```

### 특정 폴더 백업(backup)
- 자동 백업을 위한 shell script 실습
```
#!/bin/bash

src="/home/user/data"
dest="/home/user/backup"

cp -r $src $dest

echo "폴더 백업 완료"
```

### 압축 백업(backup)
- 자동 백업을 위한 shell script 실습
```
#!/bin/bash

today=$(date + %Y%m%d)
tar -cvf backup_$today.tar *.txt
echo "백업 완료"
```
- 압축 + gzip 으로 백업할 경우
```
tar -czvf backup_$today.tar gz *.txt
```
- 빠른 백업 / 내부 작업 → tar
- 파일 전송 / 저장 공간 절약 → tar.gz
- c : 새 tar 파일 생성
- v : 진행 과정 출력
- f : 파일 이름 지정
- 여러 .txt 파일을 하나의 .tar 파일로 합침
- z : gzip 압축 사용
- x : 압축 해제(extract)
- 단순히 묶인 파일만 풀기
  - tar -xvf backup_$today.tar
- 압축 해제 + 파일 풀기 동시에 수행
  - tar -xzvf backup_$today.tar.gz
- 특정 경로에 풀고 싶다면:
  - tar -xzvf backup_$today.tar.gz -C /원하는/경로

### 종합 백업 자동 스크립트
```
#!/bin/bash

# 날짜
today=$(date + %Y%m%d)

# 백업 폴더
backup_dir="backup_$today"

# 생성
mkdir -p $backup_dir

# 파일 복사
cp *.txt $backup_dir

# 압축
tar -czf $backup_dir.tar.gz $backup_dir

# 원본 폴더 삭제
rm -rf $backup_dir

echo "백업 완료: $backup_dir.tar.gz"
```

### 자동 실행(cron 설정) - 실습
- crontab = "자동 실행 시간표"
- 특정 시간에 작업을 자동으로 실행하도록 설정하는 기능
```
crontab -e

1분마다 실행
* * * * * ~/backup.sh >> ~/backup.log

2분마다 실행
*/2 * * * * ~/backup.sh >> ~/backup.log

매일밤 2시 실행
0 2 * * * ~/backup.sh >> ~/backup.log

crontab -l → 작업 상태 확인
crontab -r → 작업 삭제
---------------------------------------------
<backup.sh>

#!/bin/bash

echo "backup is finished"

chmod +x backup.sh
```

### 셸 스크립트 응용
- 현재 폴더에서 txt 파일만 출력
```
#!/bin/bash

for file in *
do
  if [[ $file == *.txt ]]
  then
    echo "텍스트 파일: $file"
  fi
done
```
- 파일이 존재하면 출력
```
#!/bin/bash

for file in file1.txt file2.txt file3.txt
do
  if [ -f "$file" ]
  then
    echo "$file 존재"
  else
    echo "$file 없음"
  fi
done
```
- 특정 확장자 파일 개수 세기
```
#!/bin/bash

count=0

for file n *
do
  if [[ $file == *.txt ]]
  then
    count=$((count + 1))
  fi
done

echo "txt 파일 개수 : $count"
```
- 에러 파일 찾기
```
#!/bin/bash

for file in *.log
do
  if grep -q "error" "$file"
  then
    echo "에러 발견 : $file"
  fi
done
```

### 검색 명령어(find)
- find는 파일 / 디렉토리를 조건으로 검색
- 
| 옵션 | 의미 |
|------|------|
| -name | 파일 이름 검색 |
| -type f | 파일 |
| -type d | 디렉토리 |
| -size | 파일 크기 |
- 특정 파일 찾기
  - find . -name "*.log"
- 확장자 검색
  - find . -name "*.log"
- 1MB 이상 파일
  - find . -size +1M
- 디렉토리만 검색
  - find . -type d

### 텍스트 검색(grep)
- grep은 파일 안의 내용(문자열)을 검색
- grep [옵션] "문자열" 파일명
- 
| 옵션 | 의미 |
|------|------|
| -i | 대소문자 무시 |
| -n | 줄 번호 출력 |
| -r | 하위 디렉토리 포함 |
| -v | 제외 검색 |
- 문자열 검색
  - grep "error" log.txt
- 줄 번호 포함
  - grep -n "error" log.txt
- 대소문자 무시
  - grep -i "error" log.txt
- 폴더 전체 검색
  - grep -r "error" .

##### ✍️작성자: 박지안
##### 🐧실습 환경: VMware - Ubuntu
##### 🗓️ 작업일: 2026-06-09
