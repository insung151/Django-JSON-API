## Renderers

TemplateResponse 인스턴스가 반환 되기전에 먼저 렌더링이 되어야한다. 렌더링 과정은 템플릿과 컨텍스트의 중간 표현을 최종 바이트스트림으로 변환해 클라이언트에게 전달한다.

drf에는 다양한 Renderer 클래스들이 있고 사용자의 필요에 따라 커스터마이징해서 사용가능하다.



View마다  Renderer 클래스 목록을 갖고 있다.

```pyth
renderer_classes = (JSONRenderer,)
```

뷰로 들어오면 drfs는 request의 내용에 따라 적절한 renderer 클래스를 결정한다. 적절한 renderer를 찾는 방법은 reqeust의 Accept헤더를 조사하는 것이다. 선택적으로 URL의 포맷 접미사를 사용하여 명시 적으로 특정 표현을 결정할 수 도 있다. (Ex : http://example.com/api/users_count.json )



##### Renderer setting

renderer의 default set은 settings.py 에서 설정가능하다.

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': (
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer',
    )
}
```

당연하 뷰마다 renderer클래스들을 설정하는것도 가능하다. 위에서 처럼(renderer_classes = ...)

function view에서는 decorator를 이용해 설정가능하다.

```python
@api_view(['GET'])
@renderer_classes((JSONRenderer,))
```



위에서 rendere_classes 들을 지정해 주었다. 이때 renderer 들의 우선순위는 어떻게 결정될까?

만약 클라이언트로 부터 받은 request가 Accept헤더를 포함하지 않고 있거나 Accept에 아무것도 명시하지 않은 경우에는 첫번째 renderer가 선택된다.

따라서 만약에 일반 웹페이지와 API 를 같이 사용하는 경우에는 TemplateHTMLRenderer 를 default renderer로 만드는 것이 좋다.	

 