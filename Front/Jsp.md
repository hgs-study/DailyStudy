## JSTL

+ JSTL에서 역순으로 출력할 경우
```
 - 페이지 상단에 
    <%@ page import="java.util.*"%> 추가 후
    
 - 리스트 선언 & Reverse 정렬
  <c:set var="item" value="${dataList}" />
  <%
      List<HashMap<String, String>> item = (List<HashMap<String, String>>) pageContext.getAttribute("item");
      Collections.reverse(item);
      pageContext.setAttribute("data", item);
  %>
  
  - 리스트 사용
    <c:forEach items="${data}" var="item" varStatus="value">
    </c:forEach>
```
