## Fetch

+ fetch 사용시 주의점(res)
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

+ fetch 사용시 주의점(err reject)
```
 - 응답으로 400등 에러를 받을 경우 fetch는 reject하지 않는다.

        .then(data => {
            if(data.ok == false)
                alert(data.message);
            else
                alert("정상적으로 처리됐습니다.")
        })
        
    async function exceptionTest() {
        const res = await fetch("/exceptionTest",{method:'get'});
        console.log(res.ok);
        const data = await res.json();
        if(res.ok == true)
            console.log(data.message);
        if(res.ok ==false)
            alert(data.message)
    }
    
   => res.status와 ok가 false or true인지 구분해서 사용하면 된다
```


+ 무한스크롤 시 주의사항
```
 $(window).scrollTop() == $(document).height() - $(window).height()
 => 현재 스크롤의 top 좌표가  > (게시글을 불러온 화면 height - 윈도우창의 height) 되는 순간
```
 - 문제 : $(document).height() =>문서 높이와 $(window).height()=> 브라우저 스크롤 높이가 똑같이 1223으로 나오는 현상 발생, 위의 코드로 작성하면 맨 하단이 아닌 맨 위 상단에서 작동함.
 - 원인 : $(window).height()가 제대로 작동하지 않음 $(document).height() 똑같이 나옴
 - 해결 : html 태그에 height:100%을 삽입 -> $(window).height() 제대로 동작

-----
## Closure
```
 - 상위 스코프의 식별자를 참조하고 있고 본인의 외부함수보다 더 오래 살아있는 것
 - 클로져가 참조하는 외부함수의 렉시컬 변수는 자유변수라고 한다.
```

+ 외부 함수와 중첩(inner)함수 예제
 ![image](https://user-images.githubusercontent.com/76584547/136181973-fa7d9cc7-b784-41e8-8beb-197ec3bb78dc.png)

1. 외부함수 (outer()) 가 실행 컨텍스트에서 제거
![image](https://user-images.githubusercontent.com/76584547/136182684-644733d5-6337-481a-93a7-8552493720cc.png)

2. ella는 중첩함수인 inner()를 참조하고 있고, 외부 함수가 실행컨텍스트에서 제거되었으나 중첩함수는 가비지 컬렉션 대상이 되지 않는다.

3. 그렇기 때문에 ella는 inner함수가 outer()함수의 렉시컬 환경을 참조 받는다
![image](https://user-images.githubusercontent.com/76584547/136183109-ea350985-11bb-46c3-aa3f-5d67d379544d.png)
