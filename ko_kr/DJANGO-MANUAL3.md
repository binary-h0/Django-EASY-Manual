<h2>4. 뷰와 템플릿</h2>

앱의 디렉토리 내의  
views.py 에서 메소드는 urls.py 에서 주는 인자값을 받을 수 있다.  

이게 무슨 말인지 예제를 보자.  

---

[앱 이름]/views.py

```python
def main(request, id):
    return HttpResponse("find %s." % id)
```

[앱 이름]/urls.py

```python
urlpatterns = [
    ...
    path('<int:id>/', views.main, name=main)
]
```

---

어떤 과정으로 전달이 되는 것인지 알아보자.  
`http://127.0.0.1:8000/[앱 이름]/5/` 로 접속하면  
[프로젝트 이름]/settings.py 에서 ROOT_URLCONF 설정에 의한  
[프로젝트 이름]/urls.py 모듈을 보게된다. urlpatterns 를 찾고  
`[앱 이름]/` 을 찾고 따라 들어가서 [앱 이름]/urls.py 에 있는 urlpatterns 에 따라  
남은 `5/` 를 처리한다. urlpatterns 에서 `<int:id>/` 와 일치함으로  
views.py 의 main 메소드에 인자 값을 넘겨준다.  

어떤 의미인지 아직도 이해를 못한다면 지금이라도 직접 따라해보자 글만 읽는 것보다 직접 해보는 것이  
이해하는데 큰 도움을 줄 수 있다.  

---

<h3> views.py 의 기능 </h3>

우리는 views 에서 데이터베이스를 사용할 수도 있고 Python Third Party 템플릿 시스템을 사용할 수도 있다.  
views.py 는 PDF 를 생성하거나 XML 을 출력하거나, 실시간으로 ZIP 파일을 만들 수도 있다.  
views.py 는 Python 의 어떠한 라이브러리도 사용할 수 있다.

<h4>하지만 여기서 중요한 점!</h4>

views.py 에 웹 코드와 디자인이 모두 하드코딩이 되어 있다면  
앱의 크기가 커질 수록 보기 매우 좋지 않고 업무를 분담하여 작업하기가 힘들 것이다...  
그래서 우리는 <strong>"분할"</strong> 을 해야한다.  
<strong>"분할"</strong> 을 위해서는 Django 의 <strong>템플릿</strong> 시스템을 이용해야 한다.

<h3> 템플릿을 만들자!!! </h3>

[앱 이름]/templates 라는 디렉토리르 만들자.  
그리고, [앱 이름]/templates/[앱 이름] 디렉토리를 하나 만들자.

그럼, "왜 templates 디렉토리안에 [앱 이름] 디렉토리를 왜 또 만드냐??" 라는 질문을 할 것 같은데.  
그에 대한 대답은 "구분하기 위해서" 이다.
자세한 설명은 다음과 같다.

```text
"Templates NameSpacing"

우리는 프로젝트를 진행할 때 하나 이상의 앱을 사용할 것이다. 
당연하게도 우리는 Django 내의 앱을 이용할 수도 다른 사람의 앱을 이용할 수도 있다.
그런데, 각각의 앱의 Templates 들을 구분하기 위해서는 그 Template 들이 
"누구"의 것인지 구분해야 할 필요가 있다.
그렇기 때문에 templates/[앱 이름] 디렉토리를 만드는 것이다.

그런데, 여기서 "templates 의 상위 디렉토리 이름을 보고 구분하면 되는것 아님?" 라고 궁금증이 생길 수도 있다.
하지만, 나는 Django 프레임워크를 만든 사람이 아니기 때문에 대답하지 않겠다.
나의 개인적인 생각이지만 아마 Templates 의 상위 디렉토리는 보지 않기 때문에 그런것 같다. (당연한 얘기인가?)
```

이제 우리는 Django 의 템플릿 시스템을 이용하기 위한 준비가 되었다.  
[앱 이름]/templates/[앱 이름] 디렉토리에 main.html 을 만들어보자 (물론 굳이 main.html 이 아니어도 좋다.)
