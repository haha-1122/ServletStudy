# JSP 내장 객체 메소드



### request

---

| 메소드                       | 설명                                                   |
| ---------------------------- | ------------------------------------------------------ |
| getParameterNames()          | 사용자가 전달한 키들을 Enumeration 객체로 반환         |
| getParameter(name)           | 사용자가 전달한 name과 일치하는 값을 반환              |
| getParameterValues(name)     | 사용자가 전달한 name과 일치하는 값을 배열형식으로 반환 |
| getCookies()                 | 클라이언트에서 전달한 쿠키를 배열 형식으로 반환        |
| getMethod()                  | 현재 요청방식을 문자열로 반환                          |
| getSession()                 | 현재 세션 객체를 반환                                  |
| getRemoteAddr()              | 클라이언트의 IP 주소를 반환                            |
| getProtocol()                | 현재 서버의 프로토콜을 문자열로 반환                   |
| setCharacterEncoding(option) | 현재 JSP로 전달되는 내용을 지정한 인코딩으로 반환      |
| getHeaderNames()             | 현재 요청이 가지는 헤더의 이름들을 반환                |
| getHeaders(name)             | 현재 요청한 헤더에서 지정한 이름의 모든 값들을 반환    |
| getQueryString()             | 현재 요청에 포함된 쿼리문자열을 반환                   |



### response

---

| 메소드                    | 설명                                     |
| ------------------------- | ---------------------------------------- |
| setContentType(type)      | content 형식을 설정                      |
| setHeader(name, value)    | 클라이언트에게 헤더로 전달할 값을 설정   |
| setDateHeader(name, date) | 클라이언트에게 헤더로 전달할 날짜를 설정 |
| sendError(status, msg)    | 클라이언트에게 에러코드와 메세지를 전달  |
| sendRedirect(url)         | 클라이언트 요청을 다른 페이지로 전달     |
| addCookie(cookie)         | 클라이언트에게 전달할 쿠키를 설정        |
| encodeURL(url)            | URL로 유효하지 않은 문자를 인코딩        |
| setStatus(sc)             | 상태 코드를 설정                         |



### out

---

| 메소드           | 설명                                               |
| ---------------- | -------------------------------------------------- |
| getBufferSize()  | output buffer의 크기를 byte로 알려준다             |
| getRemaining()   | 남아있는 버퍼의 크기중 사용가능한 비율을 알려준다. |
| clearBuffer()    | 버퍼에 있는 콘텐츠를 모두 지운다                   |
| flush()          | 버퍼를 비우고 output stream도 비운다               |
| close()          | output stream을 닫고 버퍼를 비운다                 |
| println(content) | 출력                                               |
| print()          | 출력                                               |



### session

---

| 메소드                    | 설명                                                     |
| ------------------------- | -------------------------------------------------------- |
| getID()                   | 각 접속에 대한 세션 id를 문자열형태로 반환               |
| getCreationTime()         | 세션이 생성된 시간을 밀리세컨드값으로 반환               |
| getLastAccessedTime()     | 현재 세션으로 마지막 작업한 시간을 밀리세컨드값으로 반환 |
| getMaxInactiveInterval()  | 세션 유지 시간을 초로 반환                               |
| setMaxInactiveInterval(t) | 세션 유효시간을 t에 설정된 초 값으로 설정                |
| invalidate()              | 현재 세션을 종료 세션과 관련한 값을 모두 지움            |
| getAttribute(attr)        | 문자열 attr로 설정된 세션값을 object형태로 반환          |
| setAttribute(name, attr)  | 문자열 name으로 attr을 설정                              |
| removeAttribute(name)     | 세션에 설정한 속성 값을 삭제                             |



### application

---

| 메소드                    | 설명                                                     |
| ------------------------- | -------------------------------------------------------- |
| setAttribute(name, value) | application 범위의 값 설정                               |
| getAttribute(name)        | application 범위의 값 얻기                               |
| getRealPath(path)         | 실제 물리 경로를 반환                                    |
| getResource(path)         | path 경로의 리소스를 가리키는 url을 반환                 |
| getServiceInfo()          | 현재 요청방식이 get인지 post인지를 문자열로 반환         |
| getSession()              | 현재 세션 객체를 반환                                    |
| getRemoteAddr()           | 클라이언트의 ip 주소를 반환                              |
| getProtocol()             | 현재 서버의 프로토콜을 문자열로 반환                     |
| setCharacterEncoding()    | 현재 jsp로 전달되는 내용을 지정한 문자셋으로 변환해준다. |

