## 요청과 응답(HttpServletRequest)

### HttpServletRequest 메소드

```java
@Controller
public class RequestInfo {
    @RequestMapping("/requestInfo")
    //    public static void main(String[] args) {
    public void main(HttpServletRequest request) {
        System.out.println("request.getCharacterEncoding()="+request.getCharacterEncoding()); // 요청 내용의 인코딩
        System.out.println("request.getContentLength()="+request.getContentLength());  // 요청 내용의 길이. 알수 없을 때는 -1
        System.out.println("request.getContentType()="+request.getContentType()); // 요청 내용의 타입. 알 수 없을 때는 null

        System.out.println("request.getMethod()="+request.getMethod());      // 요청 방법
        System.out.println("request.getProtocol()="+request.getProtocol());  // 프로토콜의 종류와 버젼 HTTP/1.1
        System.out.println("request.getScheme()="+request.getScheme());      // 프로토콜

        System.out.println("request.getServerName()="+request.getServerName()); // 서버 이름 또는 ip주소
        System.out.println("request.getServerPort()="+request.getServerPort()); // 서버 포트
        System.out.println("request.getRequestURL()="+request.getRequestURL()); // 요청 URL
        System.out.println("request.getRequestURI()="+request.getRequestURI()); // 요청 URI

        System.out.println("request.getContextPath()="+request.getContextPath()); // context path
        System.out.println("request.getServletPath()="+request.getServletPath()); // servlet path
        System.out.println("request.getQueryString()="+request.getQueryString()); // 쿼리 스트링

        System.out.println("request.getLocalName()="+request.getLocalName()); // 로컬 이름
        System.out.println("request.getLocalPort()="+request.getLocalPort()); // 로컬 포트

        System.out.println("request.getRemoteAddr()="+request.getRemoteAddr()); // 원격 ip주소
        System.out.println("request.getRemoteHost()="+request.getRemoteHost()); // 원격 호스트 또는 ip주소
        System.out.println("request.getRemotePort()="+request.getRemotePort()); // 원격 포트
```

**실행결과**

```text
request.getCharacterEncoding()=UTF-8
request.getContentLength()=-1
request.getContentType()=null
request.getMethod()=GET
request.getProtocol()=HTTP/1.1
request.getScheme()=http
request.getServerName()=localhost
request.getServerPort()=8080
request.getRequestURL()=http://localhost:8080/ch2/requestInfo
request.getRequestURI()=/ch2/requestInfo
request.getContextPath()=/ch2
request.getServletPath()=/requestInfo
request.getQueryString()=null
request.getLocalName()=0:0:0:0:0:0:0:1
request.getLocalPort()=8080
request.getRemoteAddr()=0:0:0:0:0:0:0:1
request.getRemoteHost()=0:0:0:0:0:0:0:1
request.getRemotePort()=4137
```

---

### 날짜 출력하기 코드

- 메소드의 매개변수로 HttpServletRequest 및 HttpServletResponse를 받기만 하면 이 객체를 톰캣이 만들어서 준다.
- 그러므로 개발자는 톰캣이 만들어준 객체를 활용하여 브라우저에 출력할 수 있다.
- 브라우저에 출력하기 위해서는 reponse 객체로부터 getWriter()메소드를 호출하여 PrintWriter 얻어야 한다.
- response.setCharacterEncoding("utf-8")이 없으면 브라우저에서 한글이 깨진다. (텍스트의 인코딩 타입을 브라우저에게 알려줘야 한다)

```java
@Controller
public class YoilTeller {
    @RequestMapping("/getYoil") // http://localhost:8080/ch2/getYoil?year=2021&month=10&day=1
    //    public static void main(String[] args) {
    public void main(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // 1. 입력
//        String year = args[0];
//        String month = args[1];
//        String day = args[2];
        String year = request.getParameter("year");
        String month = request.getParameter("month");
        String day = request.getParameter("day");

        int yyyy = Integer.parseInt(year);
        int mm = Integer.parseInt(month);
        int dd = Integer.parseInt(day);

        // 2. 처리
        Calendar cal = Calendar.getInstance();
        cal.set(yyyy, mm - 1, dd);

        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        char yoil = " 일월화수목금토".charAt(dayOfWeek);

        // 3. 출력
//        System.out.println(year + "년 " + month + "월 " + day + "일은 ");
//        System.out.println(yoil + "요일입니다.");
        response.setContentType("text/html");    // 응답의 형식을 html로 지정
        response.setCharacterEncoding("utf-8");  // 응답의 인코딩을 utf-8로 지정
        PrintWriter out = response.getWriter();  // 브라우저로의 출력 스트림(out)을 얻는다.
        out.println("<html>");
        out.println("<head>");
        out.println("</head>");
        out.println("<body>");
        out.println(year + "년 " + month + "월 " + day + "일은 ");
        out.println(yoil + "요일입니다.");
        out.println("</body>");
        out.println("</html>");
        out.close();
    }
}
```
