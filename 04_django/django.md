
## 서버 오픈
해당 프로젝트 디렉토리(폴더)에 들어가서 코드실행

>$ python manage.py runserver

24.01.09 - 사용자입력 복습/ DB 생성
### bmi 지수
- 민감정보 (키/몸무게)를 GET 메서드를 사용하게되면 사용자 입력후 URL 주소에 입력값이 다 나타남. 이를 방지하고자 method => GET > POST 를 이용함.
```python
1. bmi_in.html

<form action="/utils/bmi_out/" method='POST'>

2. veiws.py

def bmi_out(request):
    tall = float(request.POST['tall'])
    weight = float(request.POST['weight'])

```
html form action 의 method="POST"로 변경후, views.py에서 데이터를 사용자에게 보여주는 페이지의 request.GET 메서드를 > POST로 변경해줘야 함

 ### html method
1. GET : 별것 아닌 요청, 보안걱정없음, 큰 위험 없다는 뜻 
   ex) 엽서
2. POST : 별거 있, 서버가 바뀔 수 있음, 보안 이슈 있음. 
   ex) 동봉편지
 - 중요도와 관계없이 최소한의 보안절차 Csrf 검사를 함. (Cross Site Request Forgery) 사이트 간 요청위조. 해킹공격 위험.
 - 코드위치: 마스터파일 > settings > MIDDLEWARE (필터의 역할)

### CSRF 사이트 간 요청
```python
{% csrf_token %} 
```
언제 POST를 사용하는게 좋을지 내일(1/11) 수업에서 보기로.

# 데이터베이스(DB)
SQL로. 복습time : SQL > table, insert_data 파일 참조

## 1. MTV (Model Template View)
- Model : DB 담당 (!= 데이터베이스라는 뜻 아님 주의)
- Template: HTML 
- View: 함수, 중간관리자(Request, Response)
장고를 쓸 경우 Django ORM(번역기)이 python CODE를 SQL로 변환해서 DB(RDBMS)화 시켜줌
ORM (Object Relational Mapping): 객체 관계 매핑해줌, 파이썬이라는 객체를 다루듯이 데이터베이스의 SQL을 실행할 수 있음.
DB(Relational Data BMSystem)
### <정리>
1. 파이썬은 SQL 과 DB랑 통신이 원칙적으로 불가능
2. 때문에 파이썬 코드가 DB를 조작하려면 ORM(Object Relational Mapping)이 필요함
3. Object(파이썬) <=> RDBMS(DB)
4. ORM 의 역할을 하는건 많음 대표적으로 파이썬에서는 SQLalchemy 가 있음
5. 하지만 django 위에서만큼은 굳이 SQLalchemy 같이 다른거 쓸 필요 없이 django에 내장되어있는 django ORM 을 쓰면 됨

세번째 프로젝트 생성
```bash
cd ..
ls
django-admin startproject model
mv model 02_MODEL
cd 02_MODLE (또는 폴더에서 VS코드실행)
python manage.py startapp hospital

```
앱 settings.py > INSTALLED_APPS  = 'hospital', 추가
앱 models.py > 
- 장고에서는 SQLite3(경량형)을 사용. 우리는 MYSQL 사용함 나중에 갈아끼울 수 있음 참조하기. 
- SQLite3에서는 Create database 생략, 바로 table 생성
- table이 클래스 > 클래스 생성해서 ORM > 테이블생성함
```python
앱 > models.py
# 클래스 명 => 테이블 명
class Patient(models.Model):
    # 필드명 = 필드 타입
    name =   models.CharField(max_length=30)
    age =    models.IntegerField()
    weight = models.FloatField()
    height = models.FloatField()
    mbti =   models.CharField(max_length=4)
```
- 테이블명 입력하고 메서드까지 부여하고 서버 실행하면 SQL 파일이 생성됨.

> $ python manage.py makemigrations hospital (앱이름)
 
 : hospital 안에 있는 데이터 모델 스케치를 보고 만들어주세요
 
 결과: migration > 0001.initial 생성, id라는 필드 자동생성, 그외 입력한 필드 생성됨

> $ python manage.py migrate hospital
> 
: hospital 의 클래스를 장고파이썬 세상에서 => 데이터세상으로 이주하겠다.
: dbsql 파일 확인하니 컬럼들이 생성되어있음.

```
1. 테이블에 대응하는 모델 클래스 생성
2. 컬럼에 대응하는 필드 클래스 변수 생성
3. python manage.py makemigrations <app_name>
4. python manage.py migrate <app_name>
```
코드를 쓸 수 있는곳이 필요, 장고내장기능
hospital
> $ python manage.py shell
:실시간 대화형(연습용 환경), input을 매번 해야함 ex) form hospital.models import Patient , Patient()
> $ pip install django_extensions 설치
> 
: 장고에서 공식으로 만든건 아님. 장고_익스텐션즈 깃허브
> 앱 settings > Installed_apps에서 'django_extensions' 추가
 **INSTALLED_APPS**
 Native Apps(django 내장 app) , 3rd Party Apps, Local Apps 로 이루어짐
 >python manage.py 치면 쓸수있는 도구들이 나옴.
 기본장고에서 제공하는것 shell, 익스텐션 설치후 shell_plus
 **$ python manage.py shell_plus**
 : 입력하면 임포트가 자동으로 되어있음.편리함
shell_plus 사용하면 코드 다 날라감. 메모가 필요. 
종료는 Ctrl + D

```python
앱 > models.py
# 현재의 파일(02_MODEL/hospital/models.py)이
# 직접 실행($ python models.py)된 경우에만 아래 조건문 들임
if __name__ == '__main__':
    print('-------THIS IS MODELS.py------)
# 위 코드가 없으면 python manage.py runserver 할때마다 print 계속 실행됨.
# if~ 위의 코드는 어쩔수 없이 계속 실행되는데, if 아래의 코드는 자동실행 안되었으면 좋겠을때 사용!
>> $ python hospital/modes.py 입력할때(직접실행) 사용됨. runserver 같은 간접실행에서는 사용 X
```
### DB table에 할 수 있는것
데이터를 1. 생성(Create) 2.조회(Read or Retrieve) 3. 수정(Update) 4. 삭제(Delete or Destroy)
= CRUD Operations
1, 3, 4는 DB가 바뀜.
**참조**이후 CRUD 부분은 04-django > 02_MODLE > hospital > models.py 참조

24.01.10
models.py에서 삭제 후 
$ python manage.py makemigrations board
DB 반영은
$ python manage.py migrate 
![Create설명]("C:\Users\yeonjeong\TIL\04_django\Create.png")

앱 urls.py에서 path('new(페이지명)/', views.new(페이지명)) 만 만들면 거짓말! 에러남
앱 views.py에 def 페이지명 만들어줘야함.

- 개별 html 안의 태그
  
  ```python
  new.html
    {% extends "base.html" %}
    {% block content %}
    <h1>New Article</h1>
    <form action="/board/create/" method="POST">
    {% csrf_token %}
    <div>
        <label for="title">제목: </label>
        <input type="text" id="title" name='title'>
    </div>

    <div>
        <label for="content">내용: </label>
        <textarea name="content" id="content" cols="30" rows="10"></textarea>
    </div>

    {{ form }}

    <div>
        <input type="submit">
    </div>

    </form>

    {% endblock content %}
  ```

- extends > 탭 > base
- block > 탭 > content
- h1 > 탭 : 제목
- form > 탭 : 폼 태그
- div>label+input >탭 : 디비전 안에 라벨, 인풋태그 같이나옴.
- div 안에 라벨, 인풋 / div 안에 라벨, textarea / div 안에 input = "submit"
- label for, id 명칭은 통일해주면 됨 > 앱 models.py 에 DB 컬럼명에서 따옴.
- name: 서버측에서 받을때 뭐라고 받을거냐, 똑같이 통일.
- 데이터를 어디로 보내냐? form action = "/board/create/", POST 방식으로 보낸다 = csrf > 탭 : 공격에 대한 최소한의 체크포인트, 토큰 추가 
  - ★ html이라서 주소 앞에 / 붙여야함 꼭! urls.py는 장고라서 앞에 /없어도 됨.
  
```python
board > views.py
from django.shortcuts import render, redirect
from .models import Article
from .forms import ArticleForm

# 생성
def new(request):
    return render(request, 'board/new.html')

def create(request):
    # 넘어온 데이터를 form 에 입력
    print(request.POST)
    article = Article()
    article.title = 'test'
    article.content = 'testing'
    article.save()
```  
저장결과 : new.html의 폼을 통해서 /board/create/가 views.py의 create 함수를 호출, requets.POST 딕셔너리가 터미널에 찍히고 이어서 article 생성 > title, content 채우고 저장까지 완료. 
- 그런데, 위의 'test', 'testing'을 쓰는게 아니라 사용자가 게시판에서 사용자입력을 해야함. 아래처럼 바꿔주기

```python
board > views.py

    article.title = request.POST['title']
    article.content = request.POST['content']
```
지금까지는 우리가 하드타이핑 한 것으로 컨텐츠를 채웠지만, 이제는 사용자가 우리에게 요청으로 보낸 POST 데이터안에 title 키값에 해당하는걸(new.html) 통해서 채우고, textarea에 있는걸 contents에 저장함.
>> return이 없어 저장 후 화면이 에러는 나지만 DB에는 저장됨
<< 여기까지 실습2 완>>

Create 는 끝. 이제 R파트 Read.
접속많이하는 대표적인 게시판. 식물갤을 기준으로 게시판생성 생각해보기.
1. 전체게시글 목록보기 
2. 특정 게시글 상세보기

```python
board > urls.py
# Read
    # board/ => 전체 게시글 목록 조회
    path('', views.index),
    # board/1/ => 특정(pk) 게시글 상세 조회
    path('<int:pk>/', views.detail),
```

urls.py에서 read 관련 path 생성. 
특정게시글 조회시 숫자를 변수로 받아서 활용하는법: <int:pk> 변수명은 pk로 받도록 하자!여서 위와같은 코드가 됨.
도메인주소를 생성하는 과정.
여기에 맞춰서 views.py로 동작하는 함수 입력

```python
views.py
# 조회
def index(request):
   ★ articles = Article.objects.all()
    return render(request, 'board/index.html', {
        'articles': articles,
        })


def detail(request, pk):
   ★ article = Article.objects.get(pk=pk)
    return render(request, 'board/detail.html', {
        'article': article,
        })
```
게시글을 봤을때 return은 어떻게 나와야 할까?
두가지 경우 모두 html 화면을 받아봐야 함.
html을 받을때 규칙을 보면, def 다음의 함수 이름과 html 이름이 같음. 통일시켜주는게 편하다!
> 바로 index.html, board.html 파일을 만든다.
> detail에서 id=pk 또는 pk=pk 둘다 가능. django tutorial 공식문서 홈페이지에서 pk를 주로 써서 이 방식을 채택함.

![urls.py에변수명입력과정]("C:\Users\yeonjeong\TIL\04_django\urls.py에 변수명 입력과정.png")

index에는 전체 목록이라 변수명 복수형임.
마지막엔 세번째인자인 context 입력.  

html 파일로 넘어감. extends 탭 > base.html, 
block 탭 > content 
게시판 번호와 제목을 꺼내주기위해서 뭘 해야할까? for문

```html
board > templates > board > index.html
{% extends "base.html" %}

{% block content %}

<h1>All Articles</h1>

<ul>
    {% for article in articles %}
        <li>
           #{{ article.pk }}{{ article.title }}
           {{ article.pk }} | 
           <a href="/board/{{ article.pk }}/">{{ article.title }</a>
        </li>

    {% endfor %}
</ul>

{% endblock content %}
```
for 다음에 왜 index가 아닐까? 게시글 5개일 경우, 0~4 순서가 되고 enumerate라 꺼내지 못함. for idx in enumerate(articles)가 안됨.
꺼낼수있어도 데이터베이스 순서가 맞지 않음. 게시글의 id가 지워졌다가 없어지고 하면서 순서가  2, 8, 9, 10 이런 배열일 수도 있음. index적 접근이 성립되지 않는다.
> <a href="{{ article.pk }}/">{{ article.title }</a>
> / 앞에 없음 => 현재 URL 에서 뒤에 이어 붙임
> http://127.0.0.1:8000/board/ + {{ article.pk }}/
> <a href="/board/{{ article.pk }}/">{{ article.title }</a>
> / 가 맨 앞에 있음 => 도메인(127.0.0.1:8000) 뒤에 붙음
>     http://127.0.0.1:8000/ + board/{{ article.pk }}/
- 아래의 방법이 더 안전함. 언제나 board/1/의 값을 완성. 위에는 경우에 따라서 동작하지 않을 수 있음.

```html
board > templates > board > detail.html
<h1>{{ article.title }}</h1>

<p>
    {{ article.content }}
</p>

```
# 프레임워크는 흐름!
urls.py > views.py > html


