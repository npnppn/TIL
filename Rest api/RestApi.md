


4. REST API 디자인 가이드
REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있습니다.

첫 번째, URI는 정보의 자원을 표현해야 한다.
두 번째, 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

다른 것은 다 잊어도 위 내용은 꼭 기억하시길 바랍니다.

4-1. REST API 중심 규칙
1) URI는 정보의 자원을 표현해야 한다. (리소스명은 동사보다는 명사를 사용)
    GET /members/delete/1
위와 같은 방식은 REST를 제대로 적용하지 않은 URI입니다. URI는 자원을 표현하는데 중점을 두어야 합니다. delete와 같은 행위에 대한 표현이 들어가서는 안됩니다.

2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현
위의 잘못 된 URI를 HTTP Method를 통해 수정해 보면

    DELETE /members/1
으로 수정할 수 있겠습니다.
회원정보를 가져올 때는 GET, 회원 추가 시의 행위를 표현하고자 할 때는 POST METHOD를 사용하여 표현합니다.

회원정보를 가져오는 URI

    GET /members/show/1     (x)
    GET /members/1          (o)
회원을 추가할 때

    GET /members/insert/2 (x)  - GET 메서드는 리소스 생성에 맞지 않습니다.
    POST /members/2       (o)
[참고]HTTP METHOD의 알맞은 역할
POST, GET, PUT, DELETE 이 4가지의 Method를 가지고 CRUD를 할 수 있습니다.
