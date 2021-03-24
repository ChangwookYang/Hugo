## 스프링 MVC2



HTTP 요청 데이터

* Get - 쿼리 파라미터

  * /url

* POST - HSTML Form

  * content-type: application/x-www-form-urlencoded

  * 메시지 바디에 쿼리 파라미터 형식으로 전달

    * username=hello&age=20

    * 예) 회원가입, 상품주문, HTML Form 사용

    ```html
    <form action="/save" method="post">
       <input type="text" name="username" />
    </form>     
    ```

* HTTP message body

  * HTTP API에서 주로 사용 JSON, XML, TEXT
  * POST, PUT, PATCH