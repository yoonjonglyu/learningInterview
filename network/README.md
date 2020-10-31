# Network

- [HTTP의 GET 과 POST](#http의-get-과-post)

## HTTP의 GET 과 POST

> HTTP 프로토콜을 이용한 서버와의 통신 방식에는 크게 두가지 GET, POST방식이 있다.
> 일반적으로 GET 방식은 데이터를 가져오는데 사용되고, POST는 데이터를 변경하는데 사용된다. GET방식은 브라우저에서 caching된다.

1. GET : 요청 데이터가 HTTP REQUEST HEADER 부분의 URL에 담겨서 전송된다. 쿼리스트링(쿼리파라메터) URL 끝에 ?로 시작하여 =와 &으로 구분한다. 데이터 크기가 제한적이며, 보안상 URL에 다 노출되기에 적절하지않다.
2. POST : 요청 데이터가 HTTP REQUEST BODY 부분에 담겨서 전송된다. 데이터 크기가 상대적으로 크며 상대적으로 보안에 유리하다.(암호화 없이는 별차이없다.)