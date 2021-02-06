# GET & POST

 

## GET

HttpServletRequest 인스턴스의 gerPrameter() 메소드를 통해 url의 쿼리스트링을 읽어올 수 있다.
이 메소드는 반환값이 String이기 때문에 자료형을 적절히 변경해주어야 한다.

요청하는 방식은 다양하다

1.  /hello?cnt=3
2.  /hello?cnt= // ""
3.  /hello? = // null
4.  /hello // null

이렇게 3번과 4번같이 값을 명시하지 않으면 null이 반환된다.



HTML의 입력 폼을 통해 GET 처리

``` html
<body>
    <div>
        <form action="hello">
            <div>
                <lebel>인사를 몇번 받고싶나요?</lebel>
            </div>
            <div>
                <input type="text" name="cnt">
                <input type="submit" value="출력">
            </div>
        </form>
    </div>
</body>
```

form 의 action 은 servlet 매핑 주소를 적어준다.

submit 버튼을 누르면 form의 action 에 요청이 가고 브라우저는 action에 따라 url을 작성하게 된다. 그리고 input의 name에 따른 쿼리 스트링을 만들어주게 된다.

따라서 서버에 요청을 하기 위해서 html의 form 태그와 submit 버튼이 필요한 것이다.

## POST

데이터를 url이 아닌 body에 담아서 보냄

-   한글 전달 문제s

    