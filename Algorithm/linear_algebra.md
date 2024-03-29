24.01.25
데이터사이언스 스쿨 교재 
https://datascienceschool.net/intro.html
<1. 수학기호: 수열과 집합>
<2. 넘파이로 공부하는 선형대수>
2.1 데이터와 행렬
2.2 벡터와 행렬의 연산
2.3 행렬의 성질


* 선형대수학 : 많은 양의 데이터를 다루기 쉽게 해주는 수학
* 연속적인() 대신할 수 있는 수학 ()<- 수열

# 1장 수학기호
## 1.2 수열과 집합

 - index : SQL이나 프로그래밍적이지 않은 일반적 수의 나열
 - offset index : 이격, 첫번째 데이터로부터 얼마나 떨어져있는지를 나타냄. 첫번째 숫자는 첫번째 데이터와 0만큼 떨어져있어서. 그래서 0
 - 
### 수열
- 수열의 index: 순서, index
- x₁~ xn : n개의 데이터가 있다 


### 집합
- 집합에서의 index: (고유한) id 
- R = realnumber = 실수 = 실제로 존재하는 수 = 이 세상에 존재하는 모든 수 의 집합 
- x ∈ R 
실수집합 R에 포함된 실수 x
- (x₁, x₂) ∈ R*R = (x₁, x₂) ∈ R² 

### 수열의 합과 곱
- ∑ 시그마, 썸 = 합 ← 선형대수에서 많이 쓸

- ∏ 프로덕트, 파이 = 곱 ← 확률에서 많이 씀

# 2장 넘파이(Numpy)로 공부하는 선형대수
## 2.1 데이터와 행렬
### 데이터의 유형

1. 스칼라(Scalar) : 0차원 데이터, 하나의 타깃에 대해서 1종류의 데이터만 수집.
2. 벡터(Vector) : 1차원 데이터, 한 방향으로만 데이터가 증가.
   
   데이터를 모으는 방법 2가지 형식

   - 열 벡터(Colum) : 3명에 대한 키 수집, n개의 표본에 대한 한가지 데이터. **여러개의 타깃에 대한 한 종류**에 대해서 수집. (Pandas로 보면 Series에 해당)
  
   - 행 벡터(Row): 1명에 대해 키/몸무게 수집, **한개의 표본(타깃)에 대한 여러 종류의 데이터**를 수집.

3. 행렬(Matrix) : 2차원 데이터, **여러개의 타깃에 대해서 여러종류의 데이터를 수집**

| | 키 | 몸 | 세로(colum) |
|---|---|---|---|
|A	|180|83	| N |
|B	|162|59	| N |
|C	|175|72	| N |
|가로(row)| M | M |N 개수/ M 종류|


- ### 행렬로 스칼라, 벡터 표현

  - 스칼라 x ∈ R 
  - 벡터  x ∈ Rᴺ 
  - 행렬 x ∈ Rᴺˣᴹ      => N 개수 M 종류

```
[a] ∈ R ¹ˣ¹

    [x₁]
x = [x₂] ∈ R ¹ˣ¹
    [x₃]             

x = [x₁ x₂ x₃] ∈ R ¹ˣ³
```

- ML 에서 특징벡터 (Feauture Vector) => 사이킬러, 탠서플로TF, 파이토치 pytouch -> 절대 스칼라, 벡터로 처리하지 않음. 무조건 Matrix 이상부터 처리.

종류는 1종류를 유지, 개수는 n개로.
- 파이썬에서 열벡터와 행벡터 표기방법
```
    [x₁]   [[x₁]
x = [x₂] => [x₂]
    [x₃]    [x₃]]

x = [x₁ x₂ x₃] => [[x₁ x₂ x₃]]

```

1. Tensor -> 3차원 이상 Data (데이터사이언스 분야에서는 4차원 데이터 자체를 텐서로 부르기도 함. 딥러닝은 3차원이상의 데이터를 가지고 함.)
   대표적인 예시 ) 영상

- 넘파이를 사용한 벡터표현은 참고만. 직접 쓸 일이 없기때문. 사용할때 확인. 데이터와 데이터의 유사도를 확인 가능.


### 전치연산 (transpose)

행과 열을 바꿔주는 것. 
X = [x₁ x₂] n
      m  m

x ᵢ 행 번호(m) ⱼ 열 번호(n)

- xᵢⱼ  =>  xⱼᵢ

- 선형대수에서 벡터는 : 열 벡터!

행렬 x ∈ Rᴺˣᴹ   => xᵀ ∈ Rᴹˣᴺ

  => 벡터를 전치시킨다 = 열벡터를 행벡터로 바꾸는 과정

- 벡터의 전치과정

  > x ∈ Rᴺˣ¹  => xᵀ ∈ R¹ˣᴺ

  : 벡터는 전치가 불가능하다 = 전치를 하나, 하지않으나 똑같음.
여기에서는 열/행벡터 데이터종류가 지금은 상관이 없음. 수학적으로 데이터를 확인해보는 것. 표현식에 대해서만 이해.
모양에 대해서 집중.
```
[x₁]ᵀ
[x₂]  =  [x₁ x₂ x₃]
[x₃]  
```

벡터를 행렬의 형태로 표현하고 이해해야 데이터가 어떻게 바뀌어나가는지 파악하기 쉬워짐.
열벡터와 행벡터간의 차이는 상관이 없음. 왜냐하면 계산을 하기위한 변환이기 때문에 여기서는 의미가 없음. 한종류 데이터를 여러개로 모았네, 여러종류 데이터를 하나로 모았네 같은 의미는 데이터를 정리할때 필요한 개념.
수집해서 분석해야 될 때, 데이터를 쌓아놓아야 할 때의 정의.

지금부터는 계산을 할 것. 행렬과 벡터를 이용해서, 계산에 필요한 방법이라고 보면 됨.

```
                    [r₁ᵀ
X  = [C1, C2, Cₘ]  = r₂ᵀ
                     r₃ᵀ]
```

행렬을 벡터로 표현하는것
열벡터끼리 묶어서 행벡터 , 행벡터를 묶어서 열벡터로 표현가능.
트랜스포즈가 붙으면 무조건 헹벡터 , 안붙어있으면 열벡터

x ∈ Rᴺˣᴹ     X = [x₁ x₂] n
                   m  m

### 특수한 벡터와 행렬

- 영벡터(영행렬) : 열벡터의 모양으로 0이 n개가 있음. => 딥러닝할때 초기값. ex) 초기 바이러스 설정

- 일벡터 : 모든 원소가 1인 행렬. Numpy 에서 브로드캐스팅을 선형대수로 이야기할때 일벡터 사용.
[0]
[0]
[0]

- 정방행렬 : 행과 열의 개수가 똑같음. 정사각형
- 대각행렬 : 주대각방향 (우하향대각선)에만 0이 아닌 값이 차있음. 그외는 모두 0으로 채워짐. 반드시 정방행렬일 필요는 없다.
- 항등행렬 : 모든 대각성분의 값이 1 (주대각방향 값이 모두 1) 그외 0 => 역행렬하면서 다시 얘기
- 대칭행렬 : 전치를 하나, 하지않으나 똑같은 값을 가짐. 원래의 행렬이 고대로 나옴. 무조건 정방행렬만 대칭행렬이 될 수 있음.
  Sᵀ = S
  S ∈ Rᴺˣᴺ

## 2.2 벡터와 행렬의 연산
### 벡터/행렬의 덧셈과 뺄셈

요소별(element-wise) 연산을 할 때는 계산하고자 하는 행렬/벡터 모양이 정확하게 똑같아야 함.

x ∈ Rᴺˣᴹ
y ∈ Rᴺˣᴹ

### 🔴 스칼라와 벡터/행렬의 곱셈

벡터 x 또는 행렬 A의 모든 원소에 스칼라값 c를 곱하는것은 벡터x 또는 행렬 A의 모든 원소에 스칼라값 c를 곱하는 것과 같다. 모두 분배.

### 선형조합

벡터들, 행렬들 값에 스칼라값을 곱해주겠다.
모든 벡터나 행렬의 모양이 동일해야 선형조합이 가능!

>> 선형조합을 해도 그 (벡터나 행렬의)크기가 변하지 않는다. 

x₁ x₂ ··· xL ∈ Rᴺ : 벡터들이 모두 N개의 데이터를 가지고 있다 > 벡터들의 크기가 모두 N > 이 벡터들에 선형조합을 걸어도 ∈ Rᴺ
벡터 세로길이 3, 3, 4면 선형조합 성립 X
3, 3, 3으로 같아야 가능함.
c1x1 + c2x2 + c3+x3 ∈ R³

행렬을 선형조합 = 행렬

벡터를 선형조합 = 벡터

- 선형조합
❗벡터나 행렬 각각 앞에 어떤 스칼라값을 곱하고, 이것들을 모두 더하면 = 선형조합

### 벡터와 벡터의 곱셈 

-> 10 중 8,9는 내적을 뜻함.

벡터 x와 벡터 y의 내적값?
xᵀy = 내적 => "ㅓ"

앞쪽에 오는 데이터를 전치시키고, y는 그대로. 앞쪽은 행벡터의 형식, 뒤는 열벡터의 형식. "ㅓ" 자 모양을 그린다.

xᵀy : 이 모양이 보이면 내적을 하는구나 라고 생각하면 됨.

두 벡터를 내적하려면 2가지 조건이 만족되어야 함.
1) 우선 두 벡터의 차원(길이)가 같아야 함
2) 앞의 벡터가 행 벡터이고 뒤의 벡터가 열 벡터여야 한다.


원소 스칼라(x₁)와 스칼라(y₁)를 곱해줌. 각각 모조리 더해주면 됨.

```
                [y₁]
xᵀy = [x₁ x₂ x₃][y₂]  x₁y₁ +···+ xₙyₙ = (수열의 합) i=1,N ∑xᵢyᵢ 
                [y₃]


x ∈ Rᴺˣ¹      xᵀ ∈ R¹ˣᴺ
y ∈ Rᴺˣ¹

xᵀy ∈ R (스칼라)
```
- 벡터와 벡터의 내적 =>  x₁y₁ +···+ xₙyₙ 스칼라끼리의 합이 되므로 > 항상 스칼라 (나중에는 매트릭스가 되기도 함)
  
- 반대로 "ㅏ" 자로 곱할때는? 매트릭스가 됨.
(머신러닝에서 주로 사용되는 기법)

내적을 왜 할까? 정보와 정보의 조합 > 하나의 값으로 구하기 위해. 해석을 할 수 있음. 벡터와 벡터의 조합.

### 가중합
가중치를 곱하고 그걸 합한것

### 가중치(weight)

w(계수벡터) * x벡터 ⇒ wᵀx = xᵀw

가중치 w = 갯수 = 어떤 값 앞에 계수로 붙어있는 것. 미지수 뒤에 붙는 하나의 계수벡터.
갯수 > 커지면 커질수록 최종 가격이 올라감 
벡터안의 원소와 가중치의 값을 내적시킨 결과물.
내적이니 "ㅓ" 자로 곱함. 
x든 w든 어느것이든 앞에올수있음 (T트랜스포즈 가능) 내적의 순서를 바꿔도 변하지 않음.

1) 결과를 **결정**할 때 부가적인 **정보**를 의미
   - 가격을 결정할 수 있는 가격정보(가격표)
구매한 갯수A에 추가적인 정보를 줌. 곱셈이므로 연관성요약.
1) 절대값이 클 수록 결과에 영향을 많이 미친다. → 가중치가 클수록 결과의 변동이 커진다.

ex) 육포 2개 살때보다 츄파츕스 2개사는게 가격 오르는 폭이 작음. 육포보다 결과물에 영향을 덜 미친다. 왜? 가중치가 작으니까. 20개를 사도 육포1개 산거랑 같음. 

⇒ 가중치가 커지면 결과값의 변동도 커짐. 숫자가 (개수가) 조금만 바뀌어도 크게 영향을 미침.

가중치가 작으면 결과물의 영향도 적다. 가중치 크면 결과값의 영향이 많이 간다.

W 계수벡터는 변하지 않음. 언제나 고정.
몇개를 살건지는 매번 다르고 모르니까 x로 둠.
x라는 산 갯수가 바뀌니까 값이 바뀜.

### 유사도

유사도와 내적의 관계. *코사인 유사도
내적을 함으로써 유사도관계를 수치로 확인가능.

v1과 v2의 내적값 3064
v1과 v3의 내적값 1866
내적값이 더 큰 v1과 v2가 더 유사하다!  
내적 결과의 의미파악이 중요하다.

### 선형회귀모형

- 독립변수 x : 서로 영향주지 않음
- 종속변수 y : x에 의해 결정
- 계수, 가중치 : 1500, 2000

y = 1500X(오렌지) + 2000X(사과)
x오렌지, x사과끼리 독립변수

ŷ = 예측값
y = 실제 값

머신러닝 딥러닝은 가중치w와 미지수x로부터 시작함. 사용자가 구매했을때 얼마가 나올지 예측하는 수식을 만들 수 있다.

ŷ = w₁x₁ +···+ wₙxₙ
이 수식은 벡터의 내적으로 나타낼 수 있다.
ŷ = wᵀx
```
ex)w땅x땅 + w육x육 + w핫x핫 + w츄x츄
= [w1 w2 w3 w4][x₁] = wᵀx
               [x₂]
               [x₃]
```

x 미지수 = 고객들이 몇개를 살지는 모르니까
모델 = 예상되는 수치만 수식으로 써놓은것.
선형 회귀 모델

- 선형회귀의 단점 : 현실적인 데이터 100% 반영은 힘들다. 100%는 불가능하므로 적당하게는 반영할 수 있어야 함= 추세
- 이 추세를 잘 나타낼 수 있는 가중치를 구하는게 머신러닝의 목적!


### 제곱합

원소내의 모든 숫자들을 제곱해서 더함.
xᵀx

### 행렬과 행렬의 곱셈

행렬과 행렬의 내적. A*B = C
앞행렬의 열개수, 뒷행렬의 행 개수가 일치해야 내적 가능.

A ∈ Rᴺˣᴸ
B ∈ Rᴸˣᴹ
C ∈ Rᴺˣᴹ

ex) R³ˣ⁵ * R⁵ˣ⁷ = R³ˣ⁷

### 교환 법칙과 분배 법칙

1) 교환 법칙

스칼라, 벡터에서는 아래 법칙이 성립했음.
a+b=b+a
ab=ba
wᵀx = xᵀw

- 그러나 행렬과 행렬의 내적은 교환법칙이 성립하지 않음.
AB ≠ BA

A ∈ Rᴺˣᴸ
B ∈ Rᴸˣᴹ
AB ∈ Rᴺˣᴹ
BA => Rᴸˣᴹ , Rᴺˣᴸ -> M ≠ N 일 수 있음.

2) 🔴분배 법칙

스칼라에서는 성립.
a(b+c) = ab + ac = ba + ca

- 행렬은 A가 앞에있으면 항상 A가 앞에 붙어야 하고, 뒤에 있으면 뒤쪽으로만 붙을 수 있음
```
AB ≠ BA

A(B+C) = AB + AC

(A+B)C = AC + BC
```

- 전치연산도 덧셈/뺄셈에 대해 분배법칙이 성립한다.

(A+B)ᵀ=Aᵀ+Bᵀ

- 전치 연산과 곱셈의 경우에는 분배 법칙이 성립하기는 하지만 전치 연산이 분배되면서 곱셈의 순서가 바뀐다.

(AB)ᵀ=BᵀAᵀ
(ABC)ᵀ=CᵀBᵀAᵀ

24.01.26

### 곱셈의 연결

연속된 행렬의 곱셈은 계산 순서를 임의의 순서로 해도 상관없다. ABCD 순서가 바뀌진 않음. 계산이 되는 순서만 바꿔도 상관없음. 행렬 위치 자체가 바뀌는건 안됨!

ABC=(AB)C=A(BC)
ABCD=((AB)C)D=(AB)(CD)=A(BCD)

### 항등행렬의 곱셈

[1 0]
[0 1]
주대각방향 숫자만 1, 나머지는 0
a의 곱셈에 대한 항등원 a*ㅁ=a => 1
a의 덧셈에 대한 항등원 a+ㅁ=a => 0

항등행렬 = 내적에 대한 항등원
거꾸로 뒤집어도 원래행렬이 그대로 나오는것
AI = IA = A

### 행렬과 벡터의 곱

헹렬과 벡터의 내적은 벡터 (열벡터가 나온다.)

### 열 벡터의 선형조합

```
선형조합 
가중치와 미지수 x값을 내적, 곱해서 더해주는 것.
           [x₁]
[w₁ w₂ w₃] [x₂]  = w₁x₁+w₂x₂+w₃x₃
           [x₃]
```
- 벡터와 벡터의 내적 => 스칼라가 나옴.

|x₁ | x₂ | x₃ |
|---|---|---|
|1	|0|3	|
|2	|1|2	|
|0	|3|2	| 

매트릭스 X = 종류 3개, 갯수 N개
X ∈ Rᴺˣ³

원래라면 첫번째 사람은 (내적)얼마, 두번째사람은 (내적)얼마 이렇게 각각 구했었다면
이제부터 매트릭스 자체를 몽땅 넣겠다. 가격표만 옆에 붙어있도록 집어넣겠다.

```
[ 1 0 3 ] [10000]   [ㅇ]
[ 2 1 2 ] [ 2000] = [ㅇ] 
[ 0 3 2 ] [ 1500]   [ㅇ]
```
=> 열벡터의 형태로 만들어진다는걸 알 수 있음.

- '첫번째 사람이 물건에대한 각각의 가격표는 얼마다' 로도 접근가능.
- 선형조합을 이용해서도 구할 수 있음.
 행렬을 컬럼으로도 분할 할 수 있었음.

- 행렬과 벡터의 곱셈방법 2가지
1) 행렬을 열벡터로 나눠서 뒤쪽벡터와 내적을 시킬지
2) 행렬과 벡터의 내적을 할지
- 결과물은 똑같지만 계산방법은 다르기때문에 두가지 다 알고있어야 함.

이미지도 전부 매트릭스. 가중치 조절을 해서 합치게 되면 => 적절한 비율로 이미지가 합성이 됨.

### 여러 개의 벡터에 대한 가중합 동시 계산

- 행렬과 벡터를 곱하면 벡터가 된다.
- 가중치는 데이터의 종류만 상관이 있다.(종류의 갯수만큼 존재다) → 딥러닝 넘어가면 이얘기도 약간 달라짐. 앙상블, 협업이라는 기법이 있음.

```
[ X ∈ Rᴺˣᴹ ] [ w ∈ Rᴹˣ¹ ] = [ ŷ ∈ Rᴺˣ¹ ]

N이란? Data의 개수 (몇명)
가중치는 Data의 '종류갯수' 만큼
      
```
**ŷ = Xw**

어떤 미지수 행렬 X, 그에 대응하는 가중치w 를 곱해서 결과물(예측값)을 만들어냄.

### 잔차 => 오류(오차,에러)

선형 회귀분석(linear regression)을 한 결과는 가중치 벡터 w라는 형태로 나타나고 예측치는 이 가중치 벡터를 사용한 독립변수 데이터 레코드 즉, 벡터 xᵢ의 가중합 wᵀxᵢ이 된다고 했다. 예측치와 실젯값(target) yᵢ의 차이를 오차(error) 혹은 잔차(residual) eᵢ라고 한다. 이러한 잔찻값을 모든 독립변수 벡터에 대해 구하면 잔차 벡터 e가 된다.

eᵢ = yᵢ − ŷᵢ = yᵢ − wᵀxᵢ

e = y - ŷ
에러(오차벡터, 잔차벡터)는 실제 정답 `y`에서 예측값 `ŷ`를 빼주면 됨

e 잔차(오차) = y - ŷ = y - Xw

### 🔴잔차 제곱합

잔차의 크기는 잔차 벡터의 각 원소를 제곱한 후 더한 **잔차 제곱합(RSS: Residual Sum of Squares)**을 이용하여 구한다. 이 값은 eTe
로 간단하게 쓸 수 있다.
```
(i=1~N)∑e²ᵢ=(i=1~N)∑(yᵢ−wᵀxᵢ)²=eᵀe=(y−Xw)ᵀ(y−Xw)
```

어떠한 벡터에 있는 값들을 제곱해서 더해주는 것.잔차를 제곱해서 더해주는 이유? 
오차의 평균 x̄ = 0 : 다 더해버리니까 0, 오차가 없네요 라는 뜻이 되어버림. 오차는 절대적인 값이므로 음수가 나오면 안됨. 오차가 의도와는 다르게 없어질 수 있으므로 음수를 제거해야 해! 그래서 제곱 필요~

1. eᵀe : RSS, (i=1~N)∑e² 잔차를 제곱합 하는 방법
2. |e| : Absolute, 에러에 절대값을 씌우는 방법
=> 보통 1번 제곱합을 사용 . 
절대값 방법을 쓰지 않음.. 나중에 미분이 불가능해지기 때문에. 최적화가 불가능해서. 좀 비효율적이게 됨.

- 오차에 대해 제곱을 해서 더함 , 음수가 나오면 안되기 떄문에!
  
연습문제 2.2.10 
(ㅡㅣ)- (ㅡㅁㅣ)-(ㅡㅁㅣ)-(ㅡㅁㅁㅣ)
잔차 제곱합을 하면 > 스칼라가 나온다.
선형대수를 쉽게 이해하려면 값보다 모양에 집중하면 쉬움.

### 이차형식 (Quardratic form)

가지고있는 모든 원소들의 곱의 합.

wᵀxᵀxN
ㅡ ㅁㅁㅣ

xᵀx : 항상 정방행렬이 됨.
x ∈ Rᴺˣᴹ
xᵀ ∈ Rᴹˣᴺ
>> xᵀx ∈ Rᴹˣᴹ

A가 정방행렬 일때, xᵀAx를 이차형식 이라고 한다.
행벡터 정방행렬 열벡터가 붙어있는 형식을 이차형식이라고 함.
ㅡㅁㅣ => . 스칼라값

❗벡터와 행렬의 모든 원소의 곱의 합. => 스칼라가 된다.

### 🔴부분행렬

행렬과 행렬의 곱(내적) :부분행렬을 이용할 수 있음.
1) 앞의 행렬을 행벡터로 나누어 계산
   AB > [ = ]B > " ㅣㅁ"
2) 뒤에 곱해지는 행렬을 열벡터로 나누어 계산
   AB > A[ ll ]  > " ㅁㅡ "
3) 앞쪽에 오는 행렬을 열벡터로, 뒤의 행렬을 행벡터로 나누어 스칼라처럼 계산.
   AB > [ ll ][ = ] > ㅏ+ㅏ

: 계산할때 보다는 증명할때 필요.
 
## 2.3 행렬의 성질
행렬의 성질을 파악하기 위한 몇가지 방법들

### 정부호와 준정부호
- 행렬의 부호를 파악할때는 정부호, 준정부호 두가지 성질을 갖고 있음. 둘다 아닐수도 있음.
- positive definite / positive semi-definite

1) 영벡터가 아닌 모든 벡터 x에 대해 아래부등식 성립하면 행렬 A가 **양의 정부호** 라고 함.
   
   xᵀAx > 0

2) 0도 포함하면 **양의 준정부호** 라고 함.

   xᵀAx ≥ 0

> 모든 행렬에 정의할 수 있지만, 보통 **대칭행렬**에 대해서만 정의한다.
xᵀx -> 정방행렬 & 대칭행렬
(머신러닝에서 대칭행렬이 아니면 정리할 필요가 없기때문)
항등행렬은 양의 정부호
더하기로 이루어진것 > 항

와 인수분해 대박스

행렬의 크기를 정의하는 여러가지 방법, 벡터의 크기를 정의하는 여러가지 방법 중 3가지를 알아보겠음.

### 🔴행렬 놈(norm)

- 원소 안쪽에 있는것들을 전부 제곱해서 더한다음 루트를 씌운 것.

모든원소의 절대값 먼저 구하고, p의 승, 1/p 제곱을 함.
각 원소에 p승을 하고 다 더해줌 = 스칼라

p는 무조건 2만 씀! p값이 표기안되어 있으면 2라고 생각하기. 
p=2 놈은 **프로베니우스 놈(Frobenius norm)**
= ∥A∥F

놈은 항상 0보다 크거나 같다. 절대 음수가 올 수 없다.
놈은 모든 크기의 행렬에 대해서 정의할 수 있음. 벡터에 대해서도 정의할 수 있음.


❗벡터의 놈의 제곱은 그 벡터의 제곱합과 같다.

∥x∥² = ∑xᵢ² =xᵀx

정확한 놈의 정의는 아래 4가지 성질이 성립하는 행렬 연산을 말한다. 이러한 연산이 여러개 존재하기 때문에 놈의 정의도 다양하다.
1. 놈의 값은 0이상이다. 영행렬일 때만 놈의 값이 0이 된다.

       ∥A∥ ≥ 0

1.  행렬에 스칼라를 곱하면 놈의 값도 그 스칼라의 절대값을 곱한 것과 같다.

        ∥αA∥=|α|∥A∥

1. 행렬의 합의 놈은 각 행렬의 놈의 합보다 작거나 같다. 왜 커지지 않나? A+B일때 한쪽에 음수가 있을 수도 있음.

       ∥A+B∥ ≤ ∥A∥ + ∥B∥

1. 정방행렬의 곱의 놈은 각 정방행렬의 놈의 곱보다 작거나 같다. 정방행렬에서만 해당!
  
       ∥AB∥ ≤ ∥A∥ ∥B∥

이해안되면 외워서라도 숙지하기!

### 대각합

주대각방향의 값들을 전부 더한 것.
**대각합(trace)**은 정방행렬에 대해서만 정의되며 다음처럼 대각원소의 합으로 계산된다.

tr(A)=a₁₁+a₂₂+⋯+aNN=∑(i=1~N)aᵢᵢ

N차원 항등행렬의 대각합은 N
tr(IN)=N

그냥 더하다보니 음수가 될 수도 있다. (절대값을 구하거나 제곱을해서 더하는 과정이 없음)

대각합은 다음과 같은 성질이 있다. 아래의 식에서 c는 스칼라이고 A,B,C는 행렬이다.

1. 스칼라를 곱하면 대각합은 스칼라와 원래의 대각합의 곱이다.

        tr(cA)=ctr(A)

1. 전치연산을 해도 대각합이 달라지지 않는다.

        tr(Aᵀ)=tr(A)

1. 두 행렬의 합의 대각합은 두 행렬의 대각합의 합이다.

        tr(A+B)=tr(A)+tr(B)

1. 두 행렬의 곱의 대각합은 행렬의 순서를 바꾸어도 달라지지 않는다.

        tr(AB)=tr(BA)

1. 세 행렬의 곱의 대각합은 다음과 같이 순서를 순환시켜도 달라지지 않는다.

        tr(ABC)=tr(BCA)=tr(CAB)
=> 트레이스 트릭(trace trick) 미분, 딥러닝의 학습최적화 방법. 참고만 할 것.
xᵀAx=tr(xᵀAx)=tr(Axxᵀ)=tr(xxᵀA)


### 🔴행렬식 (determinant) 
수식 det(A)
정방행렬에 대해서만 계산이 가능!

만약 A ∈ R ¹ˣ¹ → 스칼라 [a] 
det(A) = det([a]) = a

- 행렬 A가 스칼라가 아니면 **여인수 전개(cofactor expansion)** 를 이용해 전개
인수: parameter 여인수: 남아있는 파라미터
=> 재귀적 호출, 똑같은 과정을 끝날때까지 계속하는 것.

det(A)=∑i=1N{(−1)i+j0Mi,j0}ai,j0 또는
det(A)=∑j=1N{(−1)i0+jMi0,j}ai0,j

위 식에서 ai,j는 A의 i행, j열 원소이고 i0(또는 j0)는 계산하는 사람이 임의로 선택한 행번호(또는 열번호)이다. 둘중에 하나를 써도 값은 똑같다.

1. determinant 값을 구할때 행이나 열 중 하나를 지정해서 잡고 시작.
2. 식 전개
3. 소행렬식 전개
4. det(A) 식에 대입

<노션정리부분 확인하기>

❗2 x 2 행렬의 행렬식
```
det([a b]) = ad−bc
   ([c d])
```
3x3은 Numpy로 돌리면 예쁘게 나옴.

**[determinant 값의 성질]**

- 전치 행렬의 행렬식은 원래의 행렬의 행렬식과 같다.
  det(AT)=det(A)

- 항등 행렬의 행렬식은 1이다.
  det(I)=1

- 두 행렬의 곱의 행렬식은 각 행렬의 행렬식의 곱과 같다.
  det(AB)=det(A)det(B)

- 역행렬 A−1은 원래의 행렬 A와 다음 관계를 만족하는 정방행렬을 말한다. I는 항등 행렬(identity matrix)이다.
행렬의 역원 = 역행렬
  A−1A=AA−1=I

- 역행렬의 행렬식은 원래의 행렬의 행렬식의 역수와 같다.
  det(A−1)=1/det(A)


## 2.4 선형 연립방정식과 역행렬

ŷ = Xw 선형 예측모형

### 선형 연립방정식
> 어떻게 하면 선형대수를 이용해 풀 수 있냐.
머신러닝을 할 수 있냐 없냐, 가능여부까지 얘기 가능.

선형회귀(모델) => 방정식을 푸는 것과 같음
정답을 맞출수 있는 가중치라는 데이터에 대한 해를 구하는게 목적
행렬을 이용해서 방정식을 어떻게 푸느냐에 대해 이야기를 할 것. 그래서 앞에서 가감법과 대입법을 얘기함. 데이터가 많아지면 두 방식을 이용하는데 한계가 있으므로, 선형대수의 역행렬을 이용해서 선형 연립방정식을 풀고자 한다!

[ A ] 
A행렬, x열벡터 = b열벡터
x미지수값은 스칼라라면 b/A로 구할 수 있음.
그러나 행렬과 벡터에는 나눗셈이라는 개념이 없음.대신에 역행렬이라는게 있어, 역행렬을 이용해서 미지수 최적의 미지수값을 구하는게 목적!

### 역행렬 (inverse matrix)

정방 행렬 A에 대한 역행렬(inverse matrix) A−1
은 원래의 행렬 A와 다음 관계를 만족하는 정방 행렬을 말한다. I는 항등 행렬(identity matrix)이다.

A⁻¹ : A의 인버스
A A⁻¹ = A⁻¹ A = I 

a x ⅟a = ⅟a x a = 1

- 역행렬이 없을수도 있음. 역행렬이 있는 행렬을 가역행렬(invertible matrix), 정칙행렬, 비특이행렬.
- 역행렬이 없는 행렬을 비가역행렬(singular matrix) 또는 특이행렬, 퇴화행렬이라고 함.
- 대각행렬의 역행렬은 대각성분의 역수로 이루어진 행렬로 만들어주면 역행렬이 된다.
λ λ¹₁ = 1
λ¹₁ = ⅟λ

### 🔴역행렬의 성질

- 전치 행렬의 역행렬은 역행렬의 전치 행렬과 같다. 따라서 대칭 행렬의 역행렬도 대칭 행렬이다.

    (Aᵀ)¹=(A¹)ᵀ

- 두 개 이상의 정방 행렬의 곱은 같은 크기의 정방 행렬이 되는데 이러한 행렬의 곱의 역행렬은 다음 성질이 성립한다.
  
    (AB)⁻¹ = B⁻¹A⁻¹
    (ABC)⁻¹ = C⁻¹B⁻¹A⁻¹

    크기가 같은 정방행렬을 내적을 시키고 역행렬을 구하게되면, 각 행렬에 역행렬을 내적한것과 같다.
    ABC 내적해서 역행렬을 구하게되면, 각각의 행렬을 거꾸로 역행렬을 곱한것과 같다.

### 역행렬의 계산

- 행렬식의 값이 0이 아니면 역행렬이 존재
    det(A) ≠ 0

- ❗det(A) = 0 , 역행렬이 존재하지 않는다.

- 역행렬이 존재하지 않으면, 연립방정식을 풀 수 없다. 해가 무수히 많거나 만족하는 해가 하나도 없거나.
=> 머신러닝 할 때 역행렬의 존재 유무를 파악하는게 중요. 보통 수집하는 데이터는 역행렬이 있을것임. 가끔 이런일이 발생.
역행렬이 존재하지 않으면 선형모델을 돌릴 수 없다.

❗연습문제 2.4.2 공식은 외울 것.
det(A) = ad - bc

**연습 문제 2.4.3**
(4)번 제외하고 꼭 풀어보기

연습 문제 2.4.4

### 셔먼모리슨 공식(Sherman–Morrison)

```
(A+uvᵀ)⁻¹ =A⁻¹−(A⁻¹uvᵀA⁻¹/1+vᵀA⁻¹u)
```
([A]+ㅣㅡ)⁻¹
A + ㅁ = A⁻¹
A의 역행렬이 존재할때, 
행렬A의 크기가 uvᵀ만큼 변화하면 = A⁻¹(역행렬)은 괄호만큼 변화한다.

### 우드베리 공식 (Woodbury)
- 셔먼모리슨은 벡터로 표현, 우드베리는 행렬로 표현한 것. 셔먼모리슨은 우드베리의 C가 1인 것 뿐. 모양만 다르지 해석하는건 똑같다.
- 변화에 대해 얘기하는 것. 안외워도 됨.

### 분할행렬의 역행렬

매트릭스를 임의로 4등분 후 자른부분의 역행렬을 구하는 것. 100x100 한것보다 효율적으로 컴퓨터를 사용할 수 있음. 데이터의 양이 굉장히 많을때, 행렬을 분할해서 분할된 공간에서 역행렬을 구하는 방식. 이런게 있구나 하고 넘어가기


## 2.4 선형 연립방정식과 역행렬

### 역행렬과 선형 연립방정식의 해

- 스칼라는 a로 나누면 되지만, 행렬은 나누기가 안되므로 역행렬로 해를 구할 수 있음.

### 선형 연립방정식

ax = b
a=계수, x=미지수 

행렬 A의 역행렬 A−1 이 존재한다면 역행렬의 정의로부터 선형 연립방정식의 해는 다음처럼 구할 수 있다. 행렬과 벡터의 순서에 주의하라.

Ax=b
A⁻¹Ax=A⁻¹b
Ix=A⁻¹b
x=A⁻¹b

### 선형 연립방정식과 선형 예측모형

`Xw = y`
내가 준비한 데이터 X, 결과물 y , 미지수 w

X :Data들(DataFrame)
↓
[Model ] w 미지수
↓
ŷ, Y :Data들에 대한 예측결과

(N개의 Data에 대한 결과물)
x가 3개면 y도 3개나옴

선형모델이라는게 선형 연립방정식을 푸는것과 다르지 않다.

❗만약 계수행렬, 여기에서는 특징행렬 X의 역행렬 X⁻¹이 존재하면 다음처럼 가중치벡터를 구할 수 있다.

w = X⁻¹y

데이터프레임에 역행렬이 존재해야 함.

### 미지수의 수와 방정식의 수

M  미지수의 수 (종류의 개수) 
N  방정식의 수 (타깃의 개수, 선형조합의 개수)

미지수의 수와 방정식의 수를 고려해 볼 때 연립방정식에는 다음과 같은 세 종류가 있을 수 있다.

1. 방정식의 수가 미지수의 수와 같다. (N=M)
2. 방정식의 수가 미지수의 수보다 적다. (N<M)→Data의 개수가 종류보다 적은 경우, 해가 무수히 많다. 그러나 비추. 불안정하기도 하고, 정확하지 않음. 정보를 파악하기 위한 근거가 부족할 수 있음.
3. 방정식의 수가 미지수의 수보다 많다. (N>M)→Data의 개수가 종류보다 많은 경우, 방정식을 만족하지 못할 수도 있음. 해가 존재하지 않을 수 있음. 

3번이 제일 많다. (세로로 긴 경우)
일반적인 경우엔 N이 M보다 큼.
* 데이터 준비할때는 세로로 긴게 제일 좋다. 

방정식 > 미지수일때
`Xw=y 를 만족하는 완벽한 w는 구할 수 없다!`
방정식을 모두 만족하는 완벽한 w 구할 수 없음. 불가능.
=> 이걸 포기해야 하는가? 이걸 위해서 최소 자승문제가 나옴.

### 🔴최소 자승문제
≈ 비슷하다 란 개념

- `N > M` 경우 완벽하게 등식(Xw=y)을 만족하는 해는 존재하지 X => 완벽은 아니더라도 비슷하게 
Xw ≈ y (w가 무한개)

- 무한개의 w중에 어떤 w를 선택할까?
  - Xw의 결과 ŷ가 정답 y와 가장 `오차없게 하는 w`를 구하자 (w 미지수 ŷ 예측값)
비슷하게 = 오차가 가장 없는 것.

Ax = ŷ ≈ y
- x는 무수히 많다. 따라서 오차가 가장 적을 x를 구하자

- 오차(잔차벡터) e = y - ŷ = ŷ - y
- 목표: 잔차벡터 e의 크기가 가장 적은 것

eᵀe = ∥e∥² = (Ax-b)ᵀ (Ax-b) 

<참고>
- y를 b로 씀(교재에서는 y대신 b로 써서 b로 대체해서 씀.)
- Ax-b 는 어디서 나왔나? e = 햇b - b = Ax-b
- 잔차의 크기는 잔차의 제곱합으로 구한다.
- 잔차제곱합 eᵀe = 놈의 제곱 ∥x∥² (벡터의 놈의 제곱은 그 벡터의 제곱합과 같다.)∥x∥² = ∑xᵢ² = xᵀx
- 잔차랑 놈, 놈제곱합 개념 다시보고옴.

> eᵀe 가 최소가 될 수 있는 x를 구하라.
> 이떄 표기 방법은 아래와 같음.
> x = argmin eᵀe * argmin (Ax-b)ᵀ(Ax-b)  

수식의 의미
- argmin`f(x)` => 어떤 x가 들어가야 함수 f(x)가 최솟값을 갖는가? 
- x = argmin`f(x)`=> 그걸 만들어주는 x를 구하세요
argmin: 최적화, 인수에 대한 조건을 만들어놓고 알맞는 값을 구하는게 목적. 함수의 인자(파라미터)
f(x):함수
y=f(x) 함수에서 x를 파라미터라고 함. 여기에 어떤x가 들어가야 최솟값을 만들어 주는지. 

- x = argmin eᵀe = `argmin (Ax-b)ᵀ(Ax-b)`
  : 예측값과 실제값의 잔차라고 할 수 있는 잔차제곱합이 최소가 되는 x를 구하시오.

`argmin (Ax-b)ᵀ(Ax-b)` => `최소 자승문제`
어떤 미지수가 설정되어야 함수(여기서는 오차 제곱합)가 최솟값이 될것인가.

문제를 풀기위한 2가지 방법
1. 역행렬 사용하기 → 의사역행렬

2. 미분을 이용한 최적화 → 경사 하강법(Gradiant Decent), 데이터값 많아지면 의사역행렬 한계가 있을때.

### 의사 역행렬

의도(psudo)를 갖고있는 역행렬

ŷ ≈ b
Ax ≈ b    >> x = A⁻¹b 불가

물결표시 안하고 이제 =로 할것임
A가 정방행렬이 아님. A ∈ Rᴺˣᴹ 강제로 정방행렬을 만들어줘야 함.

- 양 변에 Aᵀ 곱하기
- AᵀAx = Aᵀb : AᵀA는 무조건 정방행렬

- (AᵀA)가 (AᵀA)⁻¹를 갖는다면? 
  - (AᵀA)⁻¹(AᵀA)x = (AᵀA)⁻¹Aᵀb
    x = ((AᵀA)⁻¹Aᵀ)b

((AᵀA)⁻¹Aᵀ) : 정방행렬,의사역행렬 A⁺ 
x = A⁺b

## 3.1 선형대수와 해석기하의 기초

공간의미를 이해해야 함

### 벡터의 기하학적 의미

차원: (진행할 수 있는) 방향의 개수
  ex) 2차원 벡터 a : 진행방향이 2개인 벡터
[1] = a 
[2]
- 벡터 a의 값으로 표시되는 **점(point)** 또는
- 원점(0,0 영벡터)과 벡터 a의 값으로 표시되는 점을 연결한 **화살표(arrow)**

1.평행이동 = 기준 원점놓고 수치만 똑같다면 같은 벡터다. 원점에서 시작한 벡터를 항상 기준으로 놓는거. 옮겨가도 똑같음. 시작점과 수치가 다르면 다른 벡터.

2.평행하기때문에 직선간 제일 짧은거리는 언제나 직각을 이룬다.

### 벡터의 길이 

벡터 a의 길이는 놈(norm) ∥a∥으로 정의한다.

- 놈(∥a∥): 원소 안쪽에 있는것들을 전부 제곱해서 더한다음 루트를 씌운 것. p값이 2인 놈=프로베니우스 놈

### 스칼라와 벡터의 곱

- 양수 스칼라가 곱해진다면: 길이 조절
- 음수 스칼라가 곱해진다면: 반대방향(방향 180°) & 길이 모두 조절

### 🔴단위벡터
벡터의 크기를 정규화 (정해진 규격화)
길이가 1인 벡터＝ 단위벡터 
길이가 1인 단위벡터를 만드는 과정 ＝ 벡터의 정규화

MinMaxScaler ： 어떠한 데이터들을 ０～１사이로 바꾸는 행렬. 어떤 값이 들어있든 최솟값 0 최댓값 1로 만들어 주는 것. = 데이터의 정규화 과정 
데이터의 정규화 과정 ≠ 표준화 과정

벡터의 정규화 과정을 통해 단위벡터를 만들 수 있음. 단위벡터의 특징 : 항상 길이가 1.
→ 모든 벡터의 길이가 1(화살표의 길이가 무조건 1)
→ 이것의 이점? 방향에만 집중할 수 있다. 길이는 신경쓰지 않아도 됨.

⇒ `방향만 고려하기 위해 사용!` +계산의 편의성(벡터 길이의 편의성)

단위벡터를 모아놓으면 원이 됨.

영벡터가 아닌 임의의 벡터 x에 대해 다음 벡터는 벡터 x와 같은 방향을 가리키는 단위벡터가 된다.

x/∥x∥ = 1/∥x∥ *x = 1/√xᵀx *x

스칼라와 벡터의 곱으로 나타내야하기 때문에. 앞의 계수가 무조건 양수. 방향은 바뀌지 않고 길이만 바뀐다.

### 벡터의 합
벡터와 벡터의 합도 벡터가 된다. 이때 두 벡터의 합은 그 두 벡터를 이웃하는 변으로 가지는 평행사변형의 대각선 벡터가 된다.

a + b

원점에서 a만큼 이동 후, 그 위치에서  b만큼 이동

- 공간에 대한 이해를 해야하므로 단순히 합산을 하는 개념보다는 위치로 이해하기

### 벡터의 선형조합
두 벡터의 스칼라곱, 두 벡터의 합 => 아예 새로운 벡터가 됨.
여러개의 벡터를 스칼라곱을 한 후 더한것을 **선형조합**이라고 함.
- 여러개의 벡터를 스칼라곱을 한(가중치)후 더함
- 문제를 해결하기 위해, 선형조합을 통해서 벡터의 방향과 길이를 마음대로 바꿀 수 있음. 화살표를 자유롭게 조절가능 해짐 > 직선의 방정식

연습문제 3.1.1
- 선형대수학 관점에서 여러개의 데이터는 묶어낸 형태로 이해하는게 중요함.
- 역행렬로 풀어보기(가감법x)



### 벡터의 차

벡터의 차 a−b=c는 벡터 b가 가리키는 점으로부터 벡터 a가 가리키는 점을 연결하는 벡터다. 그 이유는 벡터 b에 벡터 a−b를 더하면, 즉 벡터 b
와 벡터 a−b를 연결하면 벡터 a가 되어야 하기 때문이다.
- c라는 벡터 자체가 b에서 a로 가는 벡터가 되어야 함. 
- a−b=c > a=c+b > a=b+c : b에서 c만큼 간 벡터는 a가 나와야 함.

```
a − b = c
b + c = b + (a−b) = a
```

a−b 라는 벡터는 b에서 a로 가는 벡터가 되어야 함. b에서 (a-b)만큼 간 벡터가 되어야 함. 원점에서 시작한게 아님. 우리가 보이는 공간에서 만큼은 b에서 a로 가야함.
벡터에 차에서는 굳이 원점으로 표기할 필요 없음. a-b : (벡터의 차를 보면) b에서 a로 가는 벡터구나 로 이해.

- 길이(거리)
a와 b 사이의 거리를 구하는 2가지 방식

1. 맨하튼 거리 → L1거리
    x축 1칸, y축 1칸 움직였다.

1. 유클리드(유클라디안)거리 → L2 거리
→ 대표적으로 사용됨.
    놈의 거리. 




### 직교
두 벡터 a와 b가 이루는 각이 90도이면 서로 **직교(orthogonal)**라고 하며 a⊥b로 표시한다.

cos90∘=0이므로 서로 직교인 두 벡터의 내적은 0이 된다.

aTb=bTa=0↔a⊥b


어떤 벡터 w가 있을 때 원점에서 출발한 벡터 w가 가리키는 점을 지나면서 벡터 w에 수직인 직선의 방정식을 구해보자.

이번에는 벡터 w가 가리키는 점을 지나야 한다는 조건을 없애고 단순히 

- 벡터 w에 수직인

직선 x의 방정식을 구해보자.




[x₁]ᵀ
[x₂]  =  [x₁ x₂ x₃]
[x₃]⁻¹
A ∈ Rᴺˣᴸ
ŷ = w₁x₁ +···+ wₙxₙ
wᵀx ≒  ≈
⇒