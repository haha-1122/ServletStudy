# EL (Expression Language)

JSP에서 View 의 자바 코드를 자바 코드를 사용하지 않고 값을 나타내게 하기 위한 추출 표현식이다.



``` jsp
${}
```

JSP가 실행될 때 즉시 반영된다. 또한 객체 프로퍼티 값을 꺼낼때 주로 사용한다.



``` jsp
#{}
```

시스템에서 필요하다고 판단될 때 그 값을 사용한다.

사용자 입력값을 객체의 프로퍼티에 담는 용도로 사용한다.



``` java
request.setAttribute("result", result);
RequestDispatcher dispatcher = request.getRequestDispatcher(jsp);
dispatcher.forward(request, response);
```

이렇게 Controller에서 jsp로 값을 전달해준다고 하자.



``` jsp
<%=request.getAttribute("result")%>
```

원래라면 jsp에서 이런식으로 값을 받아야 했다.

하지만 MVC2 패턴이라고 하기엔 View 에서도 자바코드가 필연적으로 생기기 마련이다. 이걸 EL 표기법을 써서 해결할 수 있다.

``` jsp
${result}
```

이렇게 훨씬 간단하게 식이 만들어진다.

이 식은 파라미터에 저장된 값이 아닌 서버의 저장소에서 가져오는 값이라는걸 잘 기억하는게 좋다.



## EL의 보관소

위의 예시에선 request에서 값을 받아서 그 값을 뿌려주는 방식이다. 저렇게 request에서 값을 받을 수도 있지만, 사실 EL은 다른 여러 보관소를 가지고 있고, 그 보관소의 우선순위 또한 존재한다. 우선순위가 존재하지 않는다면 같은 이름을 가질 경우 충돌이 일어날 것이다.



|       이름       |     보관소     |
| :--------------: | :------------: |
|    pageScope     |   JspContext   |
|   requestScope   | ServletRequest |
|   sessionScope   |  HttpSession   |
| applicationScope | ServletContext |

순서는 page 부터 application 까지다. page부터 순서대로 찾다가 원하는 값이 나오면 찾는걸 멈추고, 값이 나오지 않으면 null을 반환한다.

또한 EL표현식을 통해 특정 저장소에서만 값을 검색하도록 설정해 줄 수 있다.

``` jsp
${pageScope.variable}
```





## EL 내장 객체

위의 저장소 뿐 아니라 다른 저장소도 존재한다.

| 내장 객체        | 설명                                              |
| ---------------- | ------------------------------------------------- |
| pageScope        | Page 영역의 생명 주기에 사용되는 저장소           |
| requestScope     | Request 영역의 생명 주기에서 사용되는 저장소      |
| sessionScope     | Session 영역의 생명 주기에 사용되는 저장소        |
| applicationScope | Application 영역의 생명 주기에 사용되는 저장소    |
| param            | 파라미터 값을 저장하고 있는 저장소                |
| paramValues      | 파라미터 값을 배열로 저장하고 있는 저장소         |
| header           | Header 정보를 저장하고 있는 저장소                |
| headerValues     | Header 정보를 배열로 저장하고 있는 저장소         |
| cookie           | 쿠키 정보를 저장하고 있는 저장소                  |
| initParam        | 컨텍스트의 초기화 파라미터를 저장하고 있는 저장소 |
| pageContext      | 페이지 범위의 컨텍스트 저장소                     |



ex)

``` jsp
${param.cnt} <%--get이나 post를 통해 전송된 데이터를 받는다--%>
${paramValues.cnt[index]} <%--get이나 post를 통해 전송된 데이터를 배열로 받는다--%>

${header.option} <%--option에 해당하는 header 정보를 반환한다--%>
${header["option"]} <%--option에 사용불가능한 키워드가 들어갈 경우 이런식으로 작성한다--%>

${pageContext.request.method}
<%=pageContext.getRequest().getMethod()%> <%--위 아래가 같은 식이다. 이렇게 get과 괄호를 빼고 작성하면 좋다%>
```





## EL 연산자

EL에서 연산자 또한 사용 가능하다. 연산자는 여타 연산자들과 거의 동일하지만 축약어들도 사용할 수 있도록 설계되어 있다. 그 이유는 html자체가 태그 <>를 포함하고 있기에 가독성 등의 이유로 연산자들의 기호보다는 축약어들을 권장한다.(xml 등에서는 허용안되는 부분도 있다)

``` jsp
[].
()
not ! empty <%--empty의 경우 null이나 빈 문자열일 경우 true 반환--%>
* / div % mod
+ -
< > <= >= lt gt le ge
== != eq ne
|| or
? :
```

또한 EL에서는 정수를 정수로 나눠 실수가 생기면 실수가 반환된다.