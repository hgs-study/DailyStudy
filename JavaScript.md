## Fetch

+ fetch 사용시 주의점
```
 - 응답으로 json을 받을 경우 
 
   fetch(api, ~)
   .then((res) =>{
      return res.json();   =>res를 다시 json 형식으로 바꿔주고
   })
   .then(data =>{
      data.name       =>한번더 .then을 사용하여 하나씩 꺼내서 사용해야한다.
      date.age
   })
```


+ 무한스크롤 시 주의사항
```
 $(window).scrollTop() == $(document).height() - $(window).height()
 => 현재 스크롤의 top 좌표가  > (게시글을 불러온 화면 height - 윈도우창의 height) 되는 순간
```
 - 문제 : $(document).height() =>문서 높이와 $(window).height()=> 브라우저 스크롤 높이가 똑같이 1223으로 나오는 현상 발생, 위의 코드로 작성하면 맨 하단이 아닌 맨 위 상단에서 작동함.
 - 원인 : $(window).height()가 제대로 작동하지 않음 $(document).height() 똑같이 나옴
 - 해결 : <html> 태그에 height:100%을 삽입 -> $(window).height() 제대로 동작
