# Servlet 상태유지

웹 어플리케이션의 특성은 요청이 일어나면 자기 일을 마치면 메모리에서 사라진다는 특성을 가지고 있다.
메모리에 상주하기 않기 때문에 서블릿끼리 데이터를 공유하는 저장소가 필요하다.

상태 유지를 위한 5가지 방법

1.  application (ServletContext)
2.  session
3.  cookie
4.  hidden input
5.  querystring





#### Application 저장소 (ServeltContext)

---

웹 어플리케이션이 시작될 때 생성돼서 종료될때 까지 유지된다.
ServletContext에 저장된 데이터는 웹 어플리케이션이 실행되는 동안 모든 서블릿이 사용할 수 있다. JSP에서 application 변수를 통해 참조한다.

``` java
ServletContext application = request.GetServletContext();
application.setAttribute(name,value);
application.getAttribute(name);
```





#### Session

---

클라이언트의 최초 요청 시 생성돼서 브라우저를 닫을때 까지 유지되며 클라이언트 당 한 개가 생성된다.  보통 로그인할 때 이 보관소를 초기화하고 로그아웃하면 저장된 값을 비운다. 따라서 HttpSession에 저장된 데이터는 모든 서블릿에서 로그아웃 전까지 값을 유지할 수 있다. JSP에서 session 변수를 통해 참조한다.

웹 브라우저로부터 요청이 들어오면 그 웹 브라우저를 위한 HttpSession객체가 있는지 확인하고 없으면 새로운 HttpSession객체를 만든다. 생성된 객체는 웹 브라우저로부터 일정 시간 동안 Timeout 요청이 없으면 삭제된다. 따라서 로그인되어 있는 동안 지속적으로 사용할 데이터를 HttpSession에 저장한다.

HttpSession 객체를 무효화하는 invalidate()가 호출되어 HttpSession객체가 삭제된 후, 새로운 요청이 들어오면 HttpSessoion 객체가 새로 생성된다.

-   Session ID와 사용자 저장소 구별

    클라이언트가 브라우저를 통해 서버에 요청을 보내게 되면 WAS 가 요청을 처리하는데, 그 중 Session을 사용하는 요청이 들어오면 Session 객체를 모아둔 공간에서 클라이언트의 Session이 존재여부를 검사하게 된다.

    Session 공간에 저장할 수 있으려면 SID(Session ID)가 있어야 하는데 없을 경우엔 SID를 부여해 준다. 그리고 SID를 통해 사용자가 누구인지를 식별하게 된다. SID는 브라우저의 개발자 도구를 통해 확인할 수 있다.

    Session 공간의 저장소는 한정적이기 때문에 하나의 사용자에 대한 공간을 계속 유지시키지 않는다. 따라서 적절하게 객체를 해제해 주는 메소드들이 있다.

    ``` java
    void invalidate() // 세션 객체를 바로 해제
    void setMaxInactiveInterval(int interval) // 세션 타임아웃을 초단위로 설정
    boolean isNew() // 세션이 새로 생성되었는지 확인
    Long getCreationTime() // 1970년 1월 1일을 기준으로 세션이 시작된 시간
    long getLastAccessedTime() // 마지막 요청 시간. 기준점은 위와 같다.
    ```

    setMaxInactiveInterval 메소드는 타임아웃이 넘어갈 동안 요청이 들어오지 않으면 객체를 해제하고, 요청이 들어오면 타임아웃을 초기화 한다.



#### Cookie를 이용

---

Application 저장소나 Session 저장소는 서버쪽에 저장되지만, Cookie는 클라이언트가 저장하게 된다.  따라서 클라이언트와 연결이 끊어져도 클라이언트에 저장된 정보가 유지되어 재 방문할 때 요청정보의 헤더 안에 포함되어 서버로 전달된다.



-   쿠키 생성

    먼저 javax.servlet.http.cookie 객체를 생성한다

    ``` java
    Cookie c1 = new Cookie("쿠키이름", "쿠기값");
    ```

    쿠키 유효시간 설정은 setMaxAge()를 사용하며 단위는 초 이다.

    ``` java
    c1.setMaxAge(60*60*3);
    ```

    서버의 특정경로 요청에서만 쿠키를 전송하고자 할때 setPath() 메소드를 사용한다.
    setPath() 메소드의 인자값으로 지정하면, 지정된 경로와 그것의 하위 경로의 요청에 대해서만 클라이언트로부터 쿠키가 전송된다.

    ``` java
    c1.setPatch("/");
    ```

    쿠키는 기본적으로 전송된 서버에서만 읽어 들일 수 있지만 도메인을 설정하여 하나의 서버에서 클라이언트로 전송된 쿠키를 다른 서버에서 읽어 들일 수 있다.
    
    ``` java
    c1.setDomain("www.edu.com") // 정확히 일치하는 도메인
    c1.setDomain(".edu.com") // 서브 도메인 허용 "it.edu.com or math.edu.com"
    ```
    
    생성된 쿠키를 클라이언트로 보내기 위해서 HttpServletResponse 객체의 addCookie() 메소드를 이용한다.
    
    ``` java
    response.addCookie(c1); // 인자값에 전송할 Cookie 객체를 설정
    ```
    
    
    
-   쿠키 추출

    클라이언트로 전송된 쿠키를 서버쪽에서 읽어 들이려면 HttpServletRequest 객체의 getCookies() 메소드를 이용한다.

    ``` java
    Cookie[] list = request.getCookies();
    ```

    쿠키의 이름을 추출할 때는 Cookie 객체의 getName() 메소드 사용

    ``` java
    for(int i=0; list != null && i < list.length; i++) {
        out.println(list[i].getName() + "<br>");
    }
    ```

    쿠키의 값을 추출 할때는 Cookie 객체의 getValue() 메소드를 사용

    ``` java
    for(int i=0; list != null && i < list.length; i++) {
        out.println(list[i].getValue() + "<br>");
    }
    ```

    