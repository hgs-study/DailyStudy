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
