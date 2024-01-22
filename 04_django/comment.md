24.01.16


board > models.py
```python
from django.db import models

class Article(models.Model):
    title =   models.CharField(max_length=300)
    content = models.TextField()

    # article.comment_set.all() => 게시글이 가지고 있는 모든 댓글들 조회

class Comment(models.Model):
    # FK (외래키) => Article 모델 참조 / Article 삭제시 참조하는 댓글도 모두 삭제
    # FreingKey 는 맨 위에 적는편. 제일 중요한 정보. Article 소속이라는게 제일 중요한 것 같아! 순서는 아무런 상관이 없긴 함.
    # FK 필드 인자= 어떤 모델을 참조하는지 / on_delete 내용으로 (1:N의 관계에서 1에 해당하는)Article이 삭제되었을때 Comment는 어떻게 행동할 것인가
    article = models.ForeignKey(Article, on_delete=CASCADE)
    content = models.CharField(max_length=100)

    # comment.article => 댓글의 원 게시글 조회
```
FK 네이밍 아무렇게나 해도 됨. 그치만,
> gesigul = models.ForeignKey(Article, on_delete=CASCADE)

one_to_many에서 p.company 처럼 상품의 회사를 불러오려면? c.article (X) => gesigul이라 써두었기 때문에 
- 댓글을 하나 선택후, 댓글이 달려있는 게시글로 가고싶다면
c.gesigul 이라고 써야 함.
- article이 본인에게 달려있는 comment를 모두 보고싶다면?
  
=> article.comment_set.all()
알아서 아티클이 본인이 들고있는 1:N에 해당하는 애들을 모델명_set으로 해서 가지고 옴.

- class 함수끼리는 2줄로 띄우는것을 권장.

- 실제 데이터베이스 테이블에서는 컬럼이 어떻게 될까? id, article_id, content

```bash
$ python manage.py makemigrations board
$ python manage.py migrate board
```
```python
# article.comment_set.all() => 게시글이 가지고 있는 모든 댓글들 조회   
# comment.article => 댓글의 원 게시글 조회
```
db board_comment를 보면 article_id에 숫자가 들어감.
comment.article_id가 아니라 왜 article만 써도 되는지? 장고가 다 알아서 해주고 있음. article_id에 해당하는 article 객체를 찾아서 보여줌. 이게 더 객체지향이니까!
 - p.company (= product의 제조사 company)

id 값 까지 입력해야 할 경우
 - Company.objects.get(pk=p.company_id)

이제 돌아가서,
모델을 만들면 저장과 유효성 검사를 담당하는게 필요함.
> Form 을 만들어 줘야함.

forms.py
```python
from .models import Article, Comment

class CommentForm(forms.ModelForm):

    content = forms.CharField(min_length=2, max_length=100)

    class Meta:
        model = Comment
        fields = '__all__'
```
- 반드시 class 메타에 모델, 필즈를 꼭 써줘야 함.
- Model은 commentForm이므로 Comment 모델을 꺼내줌

models.py에서 모델(데이터베이스 테이블)도 나왔고, forms.py에서 검증할 것도 나왔음. 지금 우리가 하는 실제 앱 개발의 순서가 모델부터. 모델이 핵심. 우리가 어떤 데이터를 가지고 있을거냐.에 대한 내용. 이 데이터를 CURD 한다면? 기본적으로 모델에 대한 정의가 되어야 함. 
모델 했고, 폼 했음. 그다음은? url 후 views.py와 html 
하나의 url이 하나의 views.py 뷰 함수와 매핑이 되니까.
url을 생각하면 자연스럽게 함수가 같이 나옴. 무슨 함수를 만들어야할지도 url을 같이 생각해야 함.

댓글 생성, 조회, 삭제는 어떻게 하는거지? 생각하면서 urls 생각
댓글 생성은 어디서 하는가? detail에서. 댓글은 게시글 상세페이지에서 보여주는게 상식적이다.
urls.py
```python
# Comment Create
    # board/1/comments/create/
    paht('<int:pk>/comments/create/', views.create_comment, name='create_comment'),
# Comment Delete
    # board/????
```

create_comment : 보통 create만 적기보다는 무엇을 생성하는지도 같이 넣어주는게 좋다. 그래서 article의 create도 create => create_article이 나음. 나중에 다른 create가 더 생길 수 있기 때문에.

views.py
```python
from .models import Article, Comment
from .forms import ArticleForm, CommentForm
# HTML(<form>) 은 article detail 에서 제공
# 여기서는 검증 => 저장만 진행
def create_comment(request, pk): # board/1/comments/create/
    form = CommentForm(request.POST)

    if form.is_valid():
        comment = form.save()

    return redirect('board:detail', pk)
```
html을 제공하는 기능. 원래는 create가 form을 통해서 저장하거나 form을 가지고 화면을 주거나 둘 중 하나를 해야 함.
- from~ form이랑 comment 꺼내주기
- return 의 인텐트를 한칸 땡김 : 유효하면 저장을하고 디테일로 옴. 유효하지 않으면 저장 안하고 디테일로 올거야.


HTML은 article detail에서 제공하기로 했음
views.py 에서 detail 항목으로
```python
@require_safe
def detail(request, pk):
    # article = Article.objects.get(pk=pk)
    article = get_object_or_404(Article, pk=pk)
    form = CommentForm()
    return render(request, 'board/detail.html', {
        'article': article,
        'form': form,
    })
```
- form = CommentForm() 추가, return 3번째 인자에 'form': form, 추가.

> detail.html 가서

```html
</div>

<hr>

<div>
    <form action="" method="POST">
        {% csrf_token %}
        {{ form }}
        <button>댓글작성</button>
    </form>
</div>
```
- 전체내용 /div 태그 밑에 <hr> 가로선 하나 긋고
- div > form에 액션 비워두고, 메서드 POST, crsf 토큰, form 들어가기
- 그밑에 input type = "submit"을 해도 되지만, form 태그 안에 button 태그는 자동으로 제출이 되기 때문에 넣어줌.

저장 후 detail 구현 화면에서 어떻게 나오는지 볼 것.
: <br> 바 밑에 Article 선택지가 있음. 우리가 가지고있는 게시글이 다 나옴. 왜 나오나요?
: models.py 의 Comment는 article이라는 필드가 있는데, 외래 키(FK- 다른애와 연동되어 있음)가 써져있음. 그리고 forms.py 의 CommentForm 클래스 메타 > fields를 all이라고 썼더니, article의 전체 글을 보여주게 되는 것.
결론은 이게 말이 안됨. detail 상세보기로 들어왔으면 해당 글의 댓글을 달아야지, 다른글에 달 수 있는 선택지가 없어야함.

forms.py
```python
class CommentForm(forms.ModelForm):

    content = forms.CharField(min_length=2, max_length=100)

    class Meta:
        model = Comment
        #fields = '__all__'
        fields = ('content',)
```
튜플이니 콤마를 찍어줘야함. 리스트로 해도 상관없음
content라는 필드에 대해서만 폼한테 부탁을 하는 것.
> 새로고침 => 상세게시글 댓글창 위에 article 선택지 없어짐. content만 잘 나오고 있음.

urls.py 를 보면서 detail.html 의 댓글 form action에 뭐라고 써야할까?
폼 태그의 액션에는 Comment Create를 하기위한 url을 만들어야 함. 이 url을 만드는 변수는 appname:name
> board:create_comment
> 에러가 남.
왜 났을가, 어떻게 고쳐야 할까? pk가 있어야 함.

detail.html
```html
<div>
    <form action="{% url "board:create_comment" article.pk %}" method="POST">
        {% csrf_token %}
        {{ form }}
        <button>댓글작성</button>
    </form>
</div>
```
board의 create_comment를 할때 pk 를 넣어주는 것.
form action이 통째로 없어져도 될것같다?
비우면 어떤 뜻? <form action="" method="POST">
= 현재 detail html 페이지 로드 <form action="/board/1/" method="POST">
우리가 쓰려하는건 <form action="/board/1/comments/create/" method="POST">

댓글작성 순서
1. 댓글작성 버튼 클릭 > form action의 url을 찾아 감 > urls.py 해당 url은 views.create_comment를 실행 > views.py의 함수 form에 사용자가 넘긴 정보(request.POST)를 받아서 유효하면 저장하고 리다이렉트 할거에욤

board > forms.py에서
class Meta 코드가 있음

```python
board > forms.py
    class Meta:
        # Article 모델과 연동
        moedel = Article
        # 아래에 입력된 필드에 대해서만 검증
        fields = '__all__'


class CommentForm(forms.ModelForm):

    content = forms.CharField(min_length=2, max_length=100)

    class Meta:
        model = Comment
        fields = ('content',)
```
- ArticleForm 안에 Meta에서 fields 검증식이 있음. commentForm 보면 fields가 content만 적혀있음.

comment.article = artcle 이 필요. article 객체를 comment.article에 넣어줘야 함.
같은 예시
```python
one_to_many > models.py
    p2 = Product()
    p2.name = 'Gram Pro 360'
    p2.price = 2222700
    # 1:N 관계의 객체를 할당
    p2.company = c2
    comment.article = artcle
    p2.save()
```
comment에 article은 article 객체를 넣어줘야 함.

```python
board > views.py
def create_comment(request, pk): # board/1/comments/create/
    article = get_object_or_404(Article, pk=pk)
    form = CommentForm(request.POST)

    if form.is_valid():
        comment = form.save(commit=False)
        comment.article = article

    return redirect('board:detail', pk)
```
- article = get_object_or_404(Article, pk=pk) 추가
  => 넘어온 숫자(pk)로 comment.article 을 채워줄 수 있음. 문제는 save를 하기 전에 채워야 함.

>     if form.is_valid():
        comment = form.save(commit=False)
        comment.article = article

- comment = form.save(commit=False) : save()만 빼고 위의 내용을 모두 채워줌. 잠깐, 저장 멈춰! 코드
- comment는 form의 내용으로 채워져 있음. 저장직전에서 멈추고, 1:N 관계 설정해줌.
  form 의 제어권이 comment로 넘어와서 comment.article = article이 됨. 그래서 마지막으로 comment.save()하면 됨.
- 왜 제어권이 넘어갔나? form의 역할은 저장직전에서 멈추고, comment라는 변수에게 넘겨줌. 유효성 검사도 했는데 잠깐 저장하지 말아봐!
- 저장전에 comment 인스턴스에 form을 할당했기 때문

 - form = ArticleForm(instance=article) 이 안되는 이유: ArticleForm에 article에 대한 내용. articleForm이 기존 존재하는 article과 연관이 있다(instance=article)
