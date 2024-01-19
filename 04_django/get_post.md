24.01.15
# GET과 POST 방식 병합하기(같은 url 사용)
## 1. Create
1. 생성의 new, create / 수정 기존코드 삭제
create 코드로만 POST, GET 방식으로 들어올 경우 코드 작성.

변경 전
```python
02_MODEL > views.py
def new(request):
    form = ArticleForm()
    return render(request, 'board/new.html', {
        'form': form,
    })

def create(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('board:detail', article.pk)
    else:        
        return render(request, 'board/new.html', {
            'form': form,
    })
```

변경 후
```python
02_MODEL > views.py
def create(request):
    if request.method == 'POST':
        # 검증>저장
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('board:detail', article.pk)
        else:
            return render(request, 'board:new.html', {'form': form})

    elif request.method == 'GET':
        # HTEML 제공
        form = ArticleForm()
        return render(request, 'board:new.html', {'form': form})
```

2. board > urls.py 에 가서 new path도 지워줌

삭제내용
```python
    # board/new/ => 게시글 작성용 <form> 제공 => 'board:new'
    path('new/', views.new, name='new'),
```

3. 마스터 > base.html 에 new Article 나오는 함수 create로 바꿔줌

여기서 이제 화면보려했는데 수정부분도 지워서 url 맞춰줌
4. url.py 에서 edit 부분 삭제, update 는 주석처리

삭제내용
```python 
    # board/1/edit/ => 게시글 수정용 <form> 제공
    path('<int:pk>/edit/', views.edit, name='edit'),
```

서버돌리고 new article 작성하려니까 접속화면부터 에러남.
왜 나는 에러가나지?? veiws.py에 Modelform이 없다는데;있는디
암튼 정상적으로는 article 작성은 잘 되고, 제출하면 edit urls.py가 없어서 오류가 남.

http method - GET 방식과 POST 방식 차이
GET: 데이터를 받을 때 사용
POST: 데이터를 보낼 때 사용
왜 구분이 되어있나? >> 목적이 다름. when과 why의 차이
어떻게 보내나? http method /  url
- <form>태그의 method-"POST" 방식의 요청을 제외하고는 모두 GET방식임 (더 있겠지만 우리가 배운것 중에서는)
요청을 보내는 행위는 ?
- GET 1) 주소창 => 엔터, 2) <a>태그 => 클릭 3) redircet

new.html 에서 주석처리된 부분 삭제.

New Article > 개발자모드?F12 들어가서 form 액션을 지우고 제출해도? 저장됨.
### :form 태그의 액션이 공란이면 => 현재 내 주소창 url 그대로 사용

> 결론 => new.html에서 action 태그 부분 삭제. URL이 하나로 합쳐져서 GET, POST만 구분하게 되어서.

views.py 로 돌아감. create의 else, elif의 render 줄이 겹치게 됨. 이걸 하나로 통합할수 없을까?

```python
views.py
# 생성
def create(request):
    if request.method == 'POST':
        # 검증>저장
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('board:detail', article.pk)
        # else:
        #     return render(request, 'board/new.html', {'form': form})

    elif request.method == 'GET':
        # HTEML 제공
        form = ArticleForm()
        
    return render(request, 'board/new.html', {'form': form})
```

else 부분을 주석처리(삭제)하고, elif return 인덴트(띄어쓰기)를 한칸 앞으로 땡김.
폼 검증이 유효하지 않으면? return 맨마지막 실행됨.

결론: 이게 코드의 최종본.

update로 다시 복습해보기.
## 2. Update
```python
views.py
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            article = form.save()
            return redirect('board:detail', article.pk)

        elif request.method == 'GET':
            form = ArticleForm(instance=article)

        return render(request, 'board/edit.html', {'form': form})
```
urls.py 가서 update 주석처리부분 살리기.
edit.html 가서 edit => update로 변경
render 코드의 인자(context)가 3개였음. form 말고 article이 같이 넘겨주는 이유?
전에는 div > lable > title, content 폼으로 article.content 을 넘겨줘야 해서. 지금은 {{ form }} , views.py의 instance=article의 기존내용이 채워줌.
form action 에서 몇번 게시글로 갈지에 대한 부분이 render 3번째 인자에서 'article':article로 확인시켜줌.
> edit.html form action도 삭제

같은 url을 사용하는데, 어떤 방식으로 보내느냐에 따라 다름.
GET = 엽서 , POST = 동봉편지, 데이터베이스 변경에 영향을 줌.

### html 에서 GET을 POST 방식으로 변경
delete = POST 방식을 실행하지 않음
:detail.html 에서 delete는 a 태그 사용중.
GET/board/1/delete 
html에서 POST 방식으로 요청하려면?
form 태그 사용하기!
```html
detail.html
    <form action="{% url "board:delete" article.pk %}" method="POST">
        {% csrf_token %}
        <button onclick="return confirfm('ㄹㅇ삭제?')">
            삭제
        </button>
    </form>
```
- a태그 > form action 태그로 변경
- url 마지막에 method 추가
- form 태그밑에 csrf 토큰 추가 (POST면 무조건 따라옴.)

### form 태그 안의 button 태그
- 원래 form 태그에서 요청 발송하는, 양식에서 전송버튼은 어떻게 만들었나?

=> <input type="submit">

- form 태그 안에 있는 button 태그는 submit 을 대체. form 태그 밖의 버튼태그는 눌러도 아무런 반응 X.
  
## 3. 데코레이터 함수
- 함수를 꾸미는 용도. 대표적으로 클래스 메서드에 사용한 @classmethod 도 데코레이터이다.

create, update 함수는 GET, POST 둘다 요청이 들어올 수 있음. 
index, detail은 GET 요청만
delete 는 POST 요청만

views.py에 함수 추가
```python
from django.views.decorators.http import require_safe, require_http_methods, require_POST

각 함수들 위에 맞춰서 넣어줌
@require_http_methods : GET, POST 둘다
@require_safe : GET 요청만
@require_POST : POST 요청만
```
require_safe 는 GET을 대체함.
index 함수위에 @require_safe 함수를 써주면, 앞으로 GET 요청에서만 동작을 함. GET 요청이 아닌 다른게 들어올 경우엔 막아버림.

safe가 왜 safe냐 >> 데이터베이스에 영향을 주지 않는다고 생각하면 쉬움. 이 요청이 들어와도 괜찮다.

## 4. HTTP 상태코드 
- get_object_or_404

```python
views.py
from django.shortcuts import render, redirect, get_object_or_404 
article = Article.objects.get(pk=pk)
```
detail 함수 - 데이터를 검색할때 쓰는 코드

가령 100번째 게시글(없음) url 접속을 해보자
오류코드 : Article matching query does not exist. 100이라는 숫자에 맞는 article이 없음.

Internal Server Error
상태코드 : 내부적 에러
> 서버 개발자가 잘못했기 때문에 우리가 고칠 수 있어야 함. 고칠 수 있나? 없음.. 100 어케 만들어줘. 
ex)카페에서 김치찌개 청국장 주문한거나 마찬가지. => 안됩니다 “니가 말을 잘못한거에요.”

HTTP 상태코드! 1~5로 시작하는 코드들이 있는데, 4로 시작하는 것은 사용자 잘못.

- detail과 index 요청 받을시, 찾되 못찾으면 404에러를 보내라고 해야 함.

- 장고 숏컷함수에서 get_object_or_404 꺼내기
  > article에서 겟 오브젝트를 pk로 해보는데, 없으면 404를 만들자!

```python
@require_safe
def detail(request, pk):
    # article = Article.objects.get(pk=pk)
    article = get_object_or_404(Article, pk=pk)
    return render(request, 'board/detail.html', {
        'article': article,
    })
```
- board/100 : DosNotExist at /board/100/ 오류에서 >> Page not found(404)

> 책임소재가 달라짐!

-  detail, update, delete 함수에 404 함수로 교체함.

# 실습
```
아래 내용을 hospital 앱에서 적용해보기 
Framework 의 특성 상, 흐름을 꼭 이해하면서 바꾸기.
1. view 코드에서 new + create / edit + update
2. html 에서 <form> action 삭제 가능 이유
3. GET / POST 요청에 따라 다른 행동
4. `@require_` 로 시작하는 데코레이터
5. `get_object_or_404` 함수 사용
```
[참고](https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#imports)
