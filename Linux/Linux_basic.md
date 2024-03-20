24.03.20
리눅스 명령어를 배우는 중이다. 오늘 첫시간.

- sudo = super user do = 권한설정과 관련됨. 관리자 권한으로 실행한다는 뜻.
- 나머지는 다 '명령어 + Arguments' 형식으로 구성
- 종료는 q
  
### 명령어 설명 확인 
- man find : 상세메뉴얼들

- find -help / find -h / find  - -h: 아까보다 간략한 내용. 파라미터에 대한 설명

### 파일시스템 명령어 - 반드시 외우기

- 맨앞 '/' 는 무조건 루트를 의미 , 두번째 '/'부터는 디렉토리 구분자의 역할

```bash
pwd : print work directory

ls : 디렉토리 리스트

ls -al = ll : 상세 디렉토리,파일 리스트

ls -al -R : 디렉토리 완전상

.폴더명 : 숨김폴더 



```
여러폴더 만들기
```bash
mkdir -p dir1/dir2/dir3
mkdir -p ./dir1/dir2/dir3 해도 됨.
```
- 이전폴더 가려면 cd -
- 디렉토리 지울땐 rm -r 폴더명
- 용량이 부족할때 df : 파일시스템상에서 어떤경로가 얼마나 사용하고있는지 볼 수 있음.
- du , du -h 각 폴더 사용량 볼수있음

### ls -al (ll)
ls -al 또는 ll 누르면 나오는 10자리
d 디렉토리에 대한 3가지 권한 / 3글자씩 3묶음

소유자 / 소유자가 속한 그룹 / 소유자가 속하지 않은 그룹

r w x : 읽기 쓰기 실행  : 소유자의 권한

쓰기는 삭ㅈㅔ 권한 포함인가욤? ㅇㅇ 삭제 = 쓰기

원래 삭제없음. null로 되어있는 공간 위에 덮어씌우는 것

비어있는 byte로 씌움 → 삭제

디스크조각모음 : 비어있는 공간 모아줌

### - chmod 권한
- 이진법,,
- chmod 777 dir1 : 모든권한 다줌
- ll -R dir1 : dir1폴더내 하위디렉토리까지 모든 권한 다볼수있음

>하위디렉토리에는 모든권한 설정 안돼있음.

- 하위디렉토리까지 권한 모두 주려면? -R 붙이면 됨.
```bash
chmod -R 766 dir1
ll
ll -R dir1 : 하위디렉토리까지 권한설정됨.
```

### 파일관리명령어

mkdir sample && cd sample

&& : 명령어 두개 이어서 쓰겠다.

### echo : 출력
- echo “ 문자열 “ 출력하겠다  어디에다가? >> 여기파일
사용빈도 꽤 있음.
- echo ‘문자’ > 폴더명 : 덮어쓰기

- echo ‘문자’ > 파일명 : 파일생성

- echo ‘문자’ >> 파일명 : 파일에 문자열 append

### head, tail 
일부 내용 출력하기
- 위 10줄, 아래 10줄
- head -n 20 find_manual.txt : 20줄
- csv 파일 일부확인 할때 사용
 
### mv 
- 파일 이동 및 이름변경
```bash
# 파일 이동
ubuntu@ip-172-31-15-212:~/sample$ cd ~
ubuntu@ip-172-31-15-212:~$ mkdir sample2
ubuntu@ip-172-31-15-212:~$ ls sample
find_manual.txt  line1.txt  line2.txt  newfile
ubuntu@ip-172-31-15-212:~$ ls sample2
ubuntu@ip-172-31-15-212:~$
ubuntu@ip-172-31-15-212:~$ mv sample/newfile sample2/newfile
ubuntu@ip-172-31-15-212:~$ ls sample2
newfile
ubuntu@ip-172-31-15-212:~$ ls sample
find_manual.txt  line1.txt  line2.txt

# 파일명 변경
ubuntu@ip-172-31-15-212:~$ mv sample2/newfile sample2/gamja
ubuntu@ip-172-31-15-212:~$ ls sample2
gamja
```

### find
```bash
ubuntu@ip-172-31-15-212:~$ find . -name 'sam*'
./sample2
./sample
ubuntu@ip-172-31-15-212:~$ find . -name 'find*'
./sample/find_manual.txt
ubuntu@ip-172-31-15-212:~$ find . -name '*in*'
./.sudo_as_admin_successful
./sample/line2.txt
./sample/line1.txt
./sample/find_manual.txt
ubuntu@ip-172-31-15-212:~$
```

### which 나중에 하겠음.

### grep 
- 파일 이름 및 파일 내용을 이용해 찾기
- find는 파일 이름을 찾음.
```bash
# line 이 들어간 내용 다찾음
ubuntu@ip-172-31-15-212:~$ grep -rn "line"

# 알파벳 tion으로 끝나는거 다찾음
ubuntu@ip-172-31-15-212:~$ grep -rn "[a-zA-Z]\+tion$" .
```

### uname 
```bash
# 현재시스템 정보
ubuntu@ip-172-31-15-212:~$ uname
Linux
# all 상세정보
ubuntu@ip-172-31-15-212:~$ uname -a
Linux ip-172-31-15-212 5.15.0-1055-aws #60~20.04.1-Ubuntu SMP Thu Feb 22 15:49:52 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

### ps - 프로세스 확인 ⭐
- **PID : 프로세스 ID**, 프로세스마다 **고유**의 값

- 프로세스 : 램에 올라와있는(적재) 실행가능한 , **실행중인 프로그램**들
- disk에 a b c 프로그램 실행

Ram으로 프로그램을 끌고나옴. 각각 고유 ID를 부여해줌. like 포트처럼 고유한 숫자를 줌.

ps보다는 pid를 이용해서 제어를 함.

- ps -ef : 모든 프로세스 상세정보 나옴. 메모리/네트워크 사용량까지. (CPU)

### | (파이프)
- && : and , 앞의 코드를 한 “다음에”

파이프는 or 기호임. +을 의미

> | :  +, 명령어 두개를 조합한다

```bash
# ps -aux가 들어있는 결과 중에 worker를 찾아줌.
ubuntu@ip-172-31-15-212:~$ ps -aux | grep worker
root           7  0.0  0.0      0     0 ?        I    09:09   0:00 [worker/0:0-events]
root           8  0.0  0.0      0     0 ?        I<   09:09   0:00 [worker/0:0H-kblockd]
root          24  0.0  0.0      0     0 ?        I<   09:09   0:00 [worker/1:0H-events_highpri]
root          91  0.0  0.0      0     0 ?        I<   09:09   0:00 [worker/1:1H-kblockd]
root         109  0.0  0.0      0     0 ?        I<   09:09   0:00 [worker/0:1H-kblockd]
root         127  0.0  0.0      0     0 ?        I<   09:09   0:00 [worker/u31:0]
root         522  0.0  0.0      0     0 ?        I    09:09   0:00 [worker/1:5-cgroup_destroy]
root         907  0.0  0.1 1914712 27652 ?       Sl   09:09   0:08 /snap/amazon-ssm-agent/7983/ssm-agent-worker
root        1090  0.0  0.0      0     0 ?        I    09:38   0:00 [worker/0:1-cgroup_destroy]
root        1725  0.0  0.0      0     0 ?        I    09:43   0:00 [worker/1:0-events]
root        2139  0.0  0.0      0     0 ?        I    12:54   0:00 [worker/u30:1-events_unbound]
root        2149  0.0  0.0      0     0 ?        I    13:00   0:00 [worker/u30:0-events_power_efficient]
root        2176  0.0  0.0      0     0 ?        I    13:17   0:00 [worker/u30:2-events_unbound]
ubuntu      2294  0.0  0.0   8168   720 pts/1    S+   13:23   0:00 grep --color=auto worker
```

### sleep 일정시간동안 중지

프로세스 실행이 백그라운드에서 pid를 가진채로 돌아감?

### 🔴kill 프로세스 종료🔴 죽이세요!할때 중요 반드시 숙지 진짜 중요

- kill -9 PID번호 : 어떤 작업을 하고있든 강제로 죽여줌 = ram에서 내린다는 뜻
- kill -9 킬나인 : 종료중에 제일 강력한 시그널. 리눅스 시스템에 강제종료 누르는것과 동일. 작업관리자 > 작업끝내기(강제종료) 신호를 보내는것이라고 보면 됨.
- kill -15 PID : 이거 하던거 작업 마저하고ㅁ 끌게 시그널 우리는 안쓸것임. 프로그램 개발할게 아니기 때문에.

```bash
ubuntu@ip-172-31-15-212:~$ sleep 1000 &
[1] 2297
ubuntu@ip-172-31-15-212:~$ ps
    PID TTY          TIME CMD
   2275 pts/1    00:00:00 bash
   2297 pts/1    00:00:00 sleep
   2298 pts/1    00:00:00 ps
ubuntu@ip-172-31-15-212:~$ kill -9 2297
ubuntu@ip-172-31-15-212:~$ ps
    PID TTY          TIME CMD
   2275 pts/1    00:00:00 bash
   2301 pts/1    00:00:00 ps
[1]+  Killed                  sleep 1000
```
