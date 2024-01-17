24.01.11

# 사용자 입력
홈에서 New Article 입력에서 아무내용을 입력하지 않아도 제출이 됨. 사용자 입력을 넣을 때 어떤 검사도 해주지 않아서 그럼.

- 아무것도 입력 안할경우 
str 관련 텍스트(Varcher, text)는 빈문자열로 저장 됨.
integer나 숫자는 에러가 남.

믿으면 안되는것 1. 돈빌려달라는 사기꾼과 2.사용자 입력
실제 입력은 우리뜻대로 안됨.
애초에 우리가 원하는 모양이 아니면 받아주면 안됨. 
- 비어있으면 안됨, 제목 글자수 제한, 텍스트 글자수 제한(너무 많으면 데이터베이스 부하가 발생할 수 있으므로)

> 유효성 검사(Validation): 어떠한 검증과정을 거치겠다.
> Patient 입력이 많을수록 input 태그도 많아짐. 대신해 줄 만한게 없을지

이 두가지 해결책으로 장고가 제공해주고 있는게 Form 클래스

---

```bash
02_MODEL
$ cd board/
$ ls
$ touch forms.py
```
board앱 안의 models.py 와 forms.py 양쪽에 화면 켜두고 보기.

```python
models.py
from django.db import models

 class Article(models.Model):
    title =   models.CharField(max_length=300)
    content = models.TextField()

```

```python
forms.py
from django import forms
from .models import Article # models.py 불러오기

# ModelForm => 특정 모델과 연동되는 Form
# ModelForm의 역할 2가지
# 사용자 입력 HTML (input/textarea/..)을 생성 - views.py 의 new
# 입력 데이터 검증(Validation) => 저장까지도 대신 해줌 -views.py의 create
class ArticleForm(forms.ModelForm):
    # forms.CharField => DB 컬럼과 상관 X / 문자열을 받겠다 O
    title = forms.CharField(min_length=2, max_length=100)
    content = forms.CharField(min_length=2)

    # pyhon metaclass 가 X / django 에서 메타데이터(추가정보를) 표시하는 방법
    # 메타데이터? 데이터에 대한 데이터. html에서 언급, ex) 사진외에 사진이 찍힌 시간, 장소등 사진에 대한 추가정보
    class Meta:
        # Article 모델과 연동
        model = Article
        # 아래에 입력된 필드에 대해서만 검증
        fields = '__all__'

```
forms.py에서 class title, content => Article models.py에서 검증하려는 필드가 title, content

- Char은 Varchar가 아니라 글자란 뜻

    forms의 Char => 데이터베이스에서 쓰는  varchar가 아니라 '글자' 라는 뜻. title, content는 글자로 이루어질 것이다. 괄호안에 원하는 검증에 대해 써 줌.
    #forms.CharField(검증식)

- (forms.ModelForm)이 있고, (forms.form)도 있음.
ModelForm => 특정 모델과 연동되는 Form 
    => 여기서 models.py의 특정 모델 Article과 forms.py의 model은 연결이 되어있다 라는 뜻.

    반대로 말하면 그냥 Form은 Model과 연동이 안됨.

    > ## ModelForm의 역할 2가지

    1. 입력 데이터 검증(Validation) => 저장까지도 대신 해줌 -views.py의 create
    2. 사용자 입력 HTML (input/textarea/..)을 생성 

위의 2가지 역할을 어디에서 하는지 확인해보자

### ModelForm의 역할 확인

```python
board > vies.py
form .forms impoart ArticleForm # html 생성 (input 태그) => def new가 담당하고 있음.

def new(request):
    form = ArticleForm() # 여기서 새로운 form은 ArticleForm을 만들겠다.
    return render(request, 'board/new.html', {
        'form': form,
    })

```
- new return render에 3번째 인자 추가 = new에 추가된 form이 놀고있음 => html에서 context로 쓰려고 만든 것.
- new.html로 이동

```html
<form action="{% url "board:create" %}" method="POST">
    {% csrf_token %}
    {% comment %} <div>
        <label for="title">제목: </label>
        <input type="text" id="title" name='title'>
    </div>

    <div>
        <label for="content">내용: </label>
        <textarea name="content" id="content" cols="30" rows="10"></textarea>
    </div> {% endcomment %}

    {{ form }}

    <div>
        <input type="submit">
    </div>

</form>
```
- 원래 있던 input tag -(title, content)를 주석처리
- {{ form }} 추가

forms.py에서 class ArticleForm에 content를 textarea로 만들고 싶으면 어떻게 할까?

```python
forms.py
class ArticleForm(forms.ModelForm): 
    title = forms.CharField(min_length=2, max_length=100)
    content = forms.CharField(
        min_length=5,
        # forms.CharField 는 기본적으로 <input type="text"> 생성
        # widget이라는 옵션을 통해서 HTML 코드를 바꿀 수 있음
        widget=forms.Textarea(
            # HTML class 주는 예시 코드 (attrs라는 옵션에 딕셔너리로 추가속성을 class를 my-class로 주겠다)
            # 사용법에 관한 코드라 왜 이렇게 생겼어요? 만든 사람이 이렇게 만듬.
            attrs={'class': 'my-class'}
        )
    )

    class Meta:
        moedel = Article
        fields = '__all__'
```
- `widget`이라는 옵션을 통해 (HTML코드를 바꿀수있음) Textarea() 추가 
- title, content forms.CharField 항목으로 상세설정 안하고 주석처리 하더라도 => class meta의 article 모델을 읽어서 찰떡같이 만들어줌. 하지만 글자수 제한은 설정안됨.
- 의미있게 작성하는게 제일 중요하므로, 최소/최대 길이인 min_length, max_lenght 검증식이 들어가야 함. 거기에 추가로 HTML도 만들어주고 있음.
- 만약 bootstrap에서 쓰듯이 textarea에 class를 주고싶다면 어떻게 해야할까? => new.html에 {{ form }}으로 써놓은것이 전부이기 때문에 클래스를 줄 수 없음.
  `attrs`로 추가 가능.
- 해석: CharField글자를 입력받고 ()검증할건데 최소길이 다섯개 필요, widget겉모습을 바꿀건데 Textarea로 쓸거고, attrs(attributes)추가로 속성을 줄게 있는데 class를 my-class로 주겠다.

new.html : {{ form }} / forms.py : class ArticleForm, Meta / views.py : def new - form 추가

### views.py VS forms.py (비교)
```python
def create(request):
    article = Article()    
    article.title = request.POST['title']
    article.content = request.POST['content']
    article.save()
    return redirect('board:detail', article.pk)
---
class ArticleForm(forms.ModelForm): 
    title = forms.CharField(min_length=2, max_length=100)
    content = forms.CharField(
        min_length=5,
        widget=forms.Textarea(
            attrs={'class': 'my-class'}
        )
    )

    class Meta:
        moedel = Article
        fields = '__all__'
```

forms.py의 코드가 어색하게 느껴지는 이유 :
   
- 이전에는 vies.py create처럼 article생성, titl, content 채우고, save, redirect를 순차적으로 명확하게 써내려감. 어떻게 할꺼야? How
=> 명령형 프로그래밍 (Imperative programming)

- forms는 흐름에 대한 내용이 하나도 없이 설정만 했는데 되네? 라서. 무엇이 필요한지만 얘기. What
=> 선언형 프로그래밍(Declarative programming)

- Ctrl + ModelForm 클릭해보면 어떤 설정과 상위권에서 진행되는지 내부적인 코드 볼 수 있음.

- my-class는 우리가 직접 준 것. attrs 예시코드임. class는 어떻게 정의하는지 질문에 관한 대답을 미리 수업에서 짚어줌. 원래는 직접 html 코드로 class 정의하는데, ModelForm 때문에 {{ form }} 만 입력해야해서 추가적으로 입력이 필요한 것.

```html
<textarea class="hi" name="" id="" cols="30" rows="10"></textarea>
```

### views.py의 create 명령형 코드 > 선언형 함수 변환
forms.py와 함께 보기
```python
def create(request):
    # 넘어온 데이터를 통째로 form 에 입력
    form = ArticleForm(request.POST)
    # form 이 유효하다면, 
    # (form을 저장하는게 X, form을 통해서 article을(=form에 들어온 데이터를) 저장)
    if form.is_valid(): # forms.py에서 검증식을 검사하는 곳.
        # form을 통해 article instance 저장 <= forms.py의 class Meta > model = Article 연동을 통해
        article = form.save()
    # 저장 후 상세보기로 redirect
        return redirect('board:detail', article.pk)
    # 유효하지 않다면,
    else:        
        # 사용자 입력 데이터 + 에러 메시지를 다시 new.html 에서 보여줌
        return render(request, 'board/new.html', {
            'form': form,
    })
```
> form = ArticleForm(request.POST)
- class 를 init, 인스턴스 만들때 첫번째 인자로 넣을 수 있는게 뭐가 있냐면 data라고 나옴.
사용자가 우리에게 보낸 data 덩어리 = request.POST

- def new 에서는 빈괄호(), 쫄쫄 굶겼음 => 빈화면 나옴
- def create 는 밥을 줬을때는 사용자입력 데이터를 검증할 수 있음. 
- is_valid() 같은 메서드를 통해 유효한지 아닌지 검증 가넝한. is가 써있으면? 리턴되는 값, 또는 변수에 담긴 값이 T/F다 => forms의 검증식이 유효하다면 T, 아니면 F

> article = form.save()
- 어떻게 article을 저장할 수 있는지? => forms.py에서 class Meta에 model = Article 때문에 가능. (메타에 아티클 모델을 연동시켜놓음.)

> else: 부분

 new.html을 render 할거고,(create가 망했으니 new를 다시 보여줄 거고) 화면에 쏴 줄 form이 같이 감. 이 form은 어디서왔냐? create 첫줄코드에서 옴. 넘어온 데이터를 입력한 상태. 어차피 html 자체에서 막아주기때문에 create로 넘어오지도 못함. = > 애초에 검사할 필요가 없냐? No!!! 98% 정도는 html에서 걸러짐. 마지막 2%를 못거름. 개발자도구 F12의 요소에서 min_length, max_length를 지울 수 있음. 2% 정도의 개발 할 줄 아는 사람이면 우회할 수 있음. 아주 적지만 뚫을 확률이 있기 때문에 valid 유효성 검사를 하는것.

 => 유효성 검사로 F 결과인 폼: 입력 데이터가 채워진 채 반환됨. form = ArticleForm(request.POST) =>(request.POST=사용자 입력값=데이터)를 먹여놓은 상태(=ArticleForm)의 form 을 else render로 다시 주고있는것.

> form은 사용자 입력과 관련된 것. new와 create, edit, update에서 씀.

### views.py의 edit 명령형 코드 > 선언형 함수 변환
views.py 와 edit.html 을 보기

- edit.html 의 div 주석처리 -> title, content 부분
- {{ form }} 입력
- 
```python
views.py
def edit(request, pk):
    article = Article.objects.get(pk=pk)
    form = ArticleForm(instance=article)
    return render(request, 'board/edit.html', {
        'article': article,
        'form': form,
    })


def update(request, pk):
    article = Article.objects.get(pk=pk)
    # 사용자 데이터와 기존 article 을 같이 활용해야 함!
    form = ArticleForm(request.POST, instance=article)
    if form.is_valid():
        article = form.save()
    #article.title = request.POST['title']
    #article.content = request.POST['content']
    #article.save()
    #article.pk
    # 저장 후 상세보기로 redirect
        return redirect('board:detail', article.pk)
    else:
        return render(request, 'board/edit.html', {
            'article': article,
            'form': form,
        })
```
1. edit
   - form = ArticleForm(), 'form': form, 추가
  => 결과는 빈 칸이 나옴. new와 동일하게 비어있는 폼을 넣어서 비어있는 폼이 나옴.

   - form = ArticleForm(instance=article)
   => 특정 ArticleForm이 기존에 있음. 이걸 녹여줘야 함. 원래 썼던 기존내용 instance를 넣어줘야 합니다.

1. update
    - form = ArticleForm(request.POST) 만 넣으면 => 수정이 아니라 새로운 데이터가 생성됨. 수정은 기존에 뭔가를 가지고 해야 함. 
  
    - instance=article : 기존에 대한 내용

    - request.POST : 사용자가 넘겨준 입력 데이터 그 자체 -> 없어지면 사용자 입력이 없어짐. 사용자가 던져준 데이터를 쓰지 않겠다는 뜻.
  
    - form = ArticleForm(request.POST, instance=article) => 기존 article 내용에서 수정. ArticleForm이 사용자가 넘긴 데이터를 instance와 연동해서 수정으로 바뀜.
  

여기까지 정리. 
## 02_MODEL > hospital > forms.py 파일 생성
hospital > models.py 와 forms.py를 함께 열어서 수정.
오전에 한 CURD 실습에서 진행하는 것.

## 오전 CRUD 실습 (hospital)

### 1번

```bash
위치: 02_MODEL
$ cd hospital/
$ touch urls.py
$ mkdir -p templates/hospital
```

```python
hospital > urls.py
from django.urls import path
from . import views

app_name = 'hospital'

urlpatterns = [
    # /hospital/new/
    path('new/', views.new, name='new'),
    # /hospital/create/
    path('create/', views.create, name='create'),
    # /hospital/
    path('', views.index, name='index'),
    # /hospital/1/
    path('<int:pk>/', views.detail, name='detail'),
    # /hospital/1/edit/
    path('<int:pk>/edit/', views.edit, name='edit'),
    # /hospital/1/update/
    path('<int:pk>/update/', views.update, name='update'),
    # /hospital/1/delete/
    path('<int:pk>/delete/', views.delete, name='delete'),
]
```
> master > urls.py

```python
model > urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('board/', include('board.urls')),
    path('hospital/', include('hospital.urls')),
]
```
### 2번
master > templates > base2.html 생성
 - base.html 내용 복붙, title 태그 제목: Hospital
 - html 태그 수정시 Alt누르고 클릭 = 여러개 동시에 선택
 - hospital:new 태그 제목: New Patient

```bash
$ cd templates/hospital/
$ touch new.html index.html detail.html edit.html
```
각 html 들어가서 extends tab base2 , block tab content tab 후 복붙

> board > templates > board > detail.html
> hospital > views.py(crud)
> edit.html
> new.html
> vies.py(forms)
> index.html
> detail.html