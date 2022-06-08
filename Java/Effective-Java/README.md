## Effective-Java
+ chapter2 : 객체 생성과 파괴
  + 생성자 대신 정적 팩터리 메서드를 고려하라 
  + 생성자에 매개변수가 많다면 빌더를 고려하라
  + private 생성자나 열거 타입으로 싱글턴임을 보증하라
  + 인스턴스화를 막으려거든 private 생성자를 사용하라
  + 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
  + 불필요한 객체 생성은 피하라
  + 다 쓴 객체 참조를 해제하라
  + finalizer와 cleaner 사용을 피하라
  + try-finally보다는 try-with-resources를 사용하라

+ chapter3 : 모든 객체의 공통 메서드
  + equals는 일반 규약을 지켜 재정의하라 

+ chapter4 : 클래스와 인터페이스
  + 클래스와 멤버의 접근 권한을 최소화하라
  + public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라
  + 변경 가능성을 
