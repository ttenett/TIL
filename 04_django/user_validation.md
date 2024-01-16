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
# 입력 데이터 검증(Validation) => 저장까지도 대신 해줌
# 사용자 입력 HTML (input/textarea/..) 을 생성
class ArticleForm(forms.ModelForm):
    # forms.CharField => DB 컬럼과 상관X / 문자열을 받겠다 O
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

1.  Char은 Varchar가 아니라 글자란 뜻

    forms의 Char => 데이터베이스에서 쓰는  varchar가 아니라 '글자' 라는 뜻. title, content는 글자로 이루어질 것이다. 괄호안에 원하는 검증에 대해 써 줌.

1. (forms.ModelForm)이 있고, (forms.form)도 있음.

   1. ModelForm => 특정 모델과 연동되는 Form 
    => 여기서 models.py의 특정모델 Article과 forms.py의 model은 연결이 되어있다 라는 뜻.

    반대로 말하면 그냥 Form은 Model과 연동이 안됨.

    2. 입력 데이터 검증(Validation) => 저장까지도 대신 해줌 -views.py의 create
   
    3. 

1-11-14 9:30
