## API클래스

### 1) Wrapper Class란?
- 기본 자료타입(primitive type)을 객체로 다루기 위해서 사용하는 클래스들  
- 사용이유?
1. 래퍼 클래스는 기본 데이터 타입을 Object로 변환할 수 있다.  
2. java.util 패키지의 클래스는 객체만 처리하므로 Wrapper class는 이 경우에도 도움이 된다.  
3. ArrayList 등과 같은 Collection 프레임 워크의 데이터 구조는 기본 타입이 아닌 객체만 저장하게 되고 Wrapper Class를 사용하여 자동 방식과 언방식이 일어 난다.  
4. 멀티스레딩에서 동기화를 지원하려면 객체가 필요하다.  

### 2) Object 클래스
java.lang.object 클래스
toString() 메서드
equals() 메서드
clone() 메서드

### 3) String 클래스
java.lang.String 클래스
charAt() 메소드
compareTo() 메소드
concat()메소드
indexOf() 메소드
trim() 메소드
toLowerCase()와 toUpperCase() 메소드
substring() 메소드
split()

### 4) StringBuffer 클래스
java.lang.StringBuffer 클래스
불변 클래스와 가변 클래스
append() 메서드
capacity() 메소드
delete() 메소드
insert() 메소드

### 5) Math 클래스
java.lang.Math 클래스
Math.E와 Math.PI
random() 메소드
abs() 메소드
floor() 메소드, ceil() 메소드와 round() 메소드
max() 메소드와 min() 메소드
pow() 메소드와 sqrt() 메소드

### 6) Arrays 클래스
java.util 패키지
java.util.Arrays 클래스
binarySearch() 메소드
copyOf() 메소드
copyOfRange() 메소드
fill() 메소드
sort() 메소드
