# Servlet 한글이 깨지는 이유

servlet에서 한글이 깨지는 이유는 두 가지가 있다.

웹서버에서 클라이언트로 데이터가 보내질 때 ISO-8859-1 인코딩을 통해 데이터를 전송하게 된다.
한글은 2바이트로 되어있는데 ISO-8859-1 방식은 1byte씩 데이터를 전송하기 때문에 한글이 깨져버린다.
따라서 2byte씩 전송하는 인코딩 방식으로 전송해 주어야 한다.

그 중 UTF-8 방식이 있는데 이렇게 해주어도 한글이 깨질 수 있다. 브라우저에서 서버에서 받은 정보를 다른 인코딩 방식으로 처리했기 때문인데 이 또한 맞춰주어야 한다.

인코딩 방식을 변경해 주는 방식은 HttpServletResponse 인스턴스의 setCharacterEncoding() 메소드를 사용해주면 된다.

``` java
response.setCharacterEncoding("UTF-8");
response.setContentType("text/html; charset=utf-8"); // 컨텐츠 타입, 인코딩 방식 
```

request도 마찬가지로 인코딩 방식을 변경해주어야 한다.

이건 tomcat 환경설정에서도 바꿔줄 수 있다.
하지만 이 방식은 모든 서버의 인코딩 설정을 바꾸기 때문에 부담스러운 작업이 된다.



## 서블릿 필터

웹 서버는 WAS와 통신하고 WAS는 웹 서버의 요청에 따라 적절한 어플리케이션을 실행해 주는 역할을 하는데 현재 WAS는 톰캣이다. 

톰캣은 서블릿을 실행해주고 서블릿이 메모리상에 존재하게 되는데 메모리상에 존재하는 당시에 존재하는 공간이 있다. 그 공간을 서블릿 컨테이너라고 한다. WAS는 그 공간에 서블릿을 담아두게 된다.

서블릿 필터는 WAS와 서블릿 컨테이너와 통신하기 전 필터를 해주는 역할을 한다. 서블릿들을 컨트롤 할 수 있다는 것 이다. 

필터는 서블릿과 마찬가지로 배포서술자 방식, 애너테이션 방식으로 적용시켜줄 수 있다.

-   배포서술자

    ``` xml
    <filter>
        <filter-name></filter-name>
        <filter-class></filter-class>
    </filter>
    <filter-mapping>
        <filter-name></filter-name>
        <url-pattern></url-parttern>
    </filter-mapping>
    ```

    서블릿과 상당히 유사한데 url-pattern 부분을 이용하여 filter 클래스를 어떠한 서블릿에 적용시킬건지 결정해줄 수 있다.

 



