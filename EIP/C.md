### C언어 컴파일 과정
1. 전처리기 : 전처리 구문을 처리하는 과정 ex) #include<studio.h> 확장자 .c
2. 컴파일러 : 고수준 언어를 저수준 언어로 변환. 기계어와 가장 가까운 형태의 언어 확장자 .i / 전체를 번역하므로, 번역 시간이 오래 걸리지만, 한번 번역한 후에는 다시 번역하지 않으므로 실행속도가 빠름. C, C++, JAVA
3. 어셈블러 : 완전히 기계어로 바꾸어 주는 역할 확장자 .o
4. 링커 : 여러 개의 오브젝트 파일을 합치거나, 라이브러리를 합치는 역할 확장자 .exe
- 인터프리터
  - 한줄씩 읽어들여서 실행하는 프로그램. 번역과 실행이 동시에 이루어지므로, 별도의 실행파일이 존재하지 않음.
  - 종류: Ruby, php, javascript, Basic

# 1. 변수
   1) 변수의 개념
   - 값이 저장되는 기억공간. 저장된 데이터 값 변경될 수 있음. 정해진 자료형이 있고, 할당된 값을 가짐.
   1) 변수명 작성 규칙 
   - 사용 전에 선언해야 함
   - 영문자 또는 언더바(_)로 시작
   - 중간에 숫자와 언더바 사용가능
   - 중간 공백 사용 불가
   - 언더바 이외 특수문자 사용불가
   - 대소문자 구분
   - 예약어는 변수명으로 사용 불가
   1) 자료형(Data Type)
   2) 머지
   3) 변수의 선언
      1) 자료형 변수명 = 값;
           - int cnt // cnt 변수를 정수형으로 선언하고, 초기값 10을 대입
           - int cnt; // cnt변수를 정수형으로 선언하고, 초기값이 없을 때, 쓰레기값이 대입
           - char a = 'C'; // a변수를 문자형으로 선언하고 초기값 C를 대입

# 2. 연산자
   1) 산술 연산자 : + - * / % ++ --
   2) 관계 연산자 : >크다(1,true) >=크거나 같다(1,true) <작다(0,false) <=작거나 같다(1,true) == 같다(0,false) != 같지 않다(1,true)
      1) C(1,0), JAVA(true,false), Python(True,False)
   3) 논리 연산자 : && AND 둘다참이어야함.(0,false) , || OR 둘중 하나만 참 (1,true), ! NOT (0,false)
   4) 비트 연산자 : & 비트 AND , | OR, ~ NOT, ^ XOR(안똑같으 참), <<좌 이동 오른쪽추가00, >> 우 비트이동 오른쪽 두자리 삭제.
   - 비트낫 연산 ~num : num이 양수이면 절댓값 +1에 (-)붙이기. 음수이면 절댓값에서-1해주고 양수로 변환. 원래는 1의보수, 2의보수로 이진수로 해당값을 만든후, 1의보수(이진수의 반대) / 2의보수는 1을 더한 값으로 찾는 것.
   5) 삼항 연산자 ? : 10>3?10:3 조건?참:거짓 결과는 10
   6) 대입 연산자 : += a=a+1, -=, *=, /=, %=
  - 연산자 우선순위 : 단항(++, --, !), 산술, 시프트(<<,>>), 관계, 비트, 논리, 삼항, 대입

# 입출력 함수
- #include <studio.h> : 스튜디오헤더파일의 라이브러리 불러올 것임.
1) printf()/ scanf() 출력 / 입
   1) 가장 많이 사용하는 표준 입출력 함수로, <studio.h> 헤더파일에 정의
   2) 

- #define VALUE1 1 : 매크로, 앞으로 나오는 VALUE1 값은 모두 1 
 

for, if 문 : 첫실행문, 조건문, 증가문, 수행문으로 이루어짐
포인터변수 * : 해당 주소값을 변수로 사용함
- strcpy : 복사, strncpy(ㅇ,ㅇ,ㅇ) :마지막 값의 길이만큼 복사. 
배열 
- struct : 변수 여러개를 포함하여 구조체 생성
- malloc : 동적으로 메모리에 할당하는 함수.

- 공용체는 메모리를 공유한다. 가장 큰 기억공간 하나만 사용. int, float = 4byte, double = 8byte

메모리에 프로그래밍
코드-소스코드, 데이터-전역변수, 정적변수, 힙-동적으로 할당한 것들, 스택-지역변수
static : 정적변수, 전역+지역변수의 특징 모두 가지고 있음. 전역변수? 프로그래밍이 끝날때까지 메모리상에 남아있음. 초기화 한번만 시킴. 지역변수? 해당하는 함수내에서만 실행. 처음 선언되고 나서부터는 초기화 되지 않고, 이전값 유지. 두번째 호출부터는 실행되지 않음.

- 함수구조
반환타입 함수명(인자들){
   수행할 작업1
   수행할 작업2
}

void 타입 : 반환하지 않아도 됨.

return 0 : 다른 함수에서 선언된거면, 이 값을 넘겨주면 됨. main함수에서의 맨 마지막줄 return 0은 프로그램이 종료되었다는 뜻. main함수 중간에도 return 0 이면 거기서 종료.

continue : 다시 위로 올라가서 반복 수행함. 무한 반복이 될 수 있음.

define : 매크로 변수, 앞으로 나올 모든 변수값은 고정

scanf : 입력받는 함수, 두번째 인자에 주솟값을 받는다.

little endian 출력방법: 거꾸로 출력.

-49를 3.1f로 표현: 3.1f일때 49는 9.0이 아니라, 정수부분을 다 표현해야 하므로 49.0으로 표기함.
-그냥 %f면, 49.000000 6개표현함.

```C
#include <stdio.h>
#include <unistd.h> : fork라는 함수를 사용할 수 있는 라이브러리

int main(void) {
   int x = 0;
   fork();
   x = 1;
   printf("%d\n,x);
   return 0;
}
```
fork : 부모와 같은 변수를 만듦. 부모에서 변수 할당하면 자식프로세스에서도 똑같이 변경됨. 함수 출력하면 부모와 자식 둘다 출력. 한번 더 나옴.
   - fork의 리턴값: 양수-부모, 0-자식, 음수-실패했다는 뜻.
   - 부모 조건부터 수행함. 그런데 부모조건에 wait가 있으면 자식 조건부터 수행한 뒤 부모조건 수행함.

# 출력형식
## 출력변환 기호
- %d : 부호 있는 10진수 출력
- %f : 고정 소수점으로 출력
- %c : 문자 출력
- %s : 문자열 출력
- %x : 16진수 출력
- %o : 8진수 출력

- DB 설계 : 물리적 설계, 논리적 설계
- 외부스키마, 개념스키마, 내부스키마 