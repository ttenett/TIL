
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

```
1. 테이블에 대응하는 모델 클래스 생성
2. 컬럼에 대응하는 필드 클래스 변수 생성
3. python manage.py makemigrations <app_name>
4. python manage.py migrate <app_name>
```
> $ python manage.py shell