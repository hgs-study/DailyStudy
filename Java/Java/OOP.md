## OOP

#### 객체
---
```
  "기능"으로 정의한 클래스
```

+ 객체일까?
```JAVA
  public class Member{
    private String id;
    private String name;
    
    public void setId(String id){
      this.id = id;
    }
    public String getId(){
      return id;
    }
    public void setName(String name){
      this.name = name;
    }
    public String getName(String name){
      return name;
    }
```
+ 해당 클래스는 데이터(name,id)를 접근하고 조회하기만 하기 때문에 "데이터"클래스라고 불림
+ "기능"이 없으므로 객체보단 데이터 클래스
