
## 서버 오픈
해당 프로젝트 디렉토리(폴더)에 들어가서 코드실행

>$ python manage.py runserver

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





