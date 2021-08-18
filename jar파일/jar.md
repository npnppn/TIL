JAR(Java Archive)
JAR는 ZIP 파일 포맷으로 이루어진 압축 파일 포맷이다. 여러 개의 자바 클래스 파일과, 클래스들이 이용하는 관련 리소스(텍스트, 그림 등) 및 메타데이터를 하나의 파일로 모아준다.

즉 JAR는 자바 애플리케이션을 구성하는 것들을 단일 파일로 묶어 압축한 형태의 파일로, 한 차례의 요청으로 애플리케이션 전체를 다운로드할 수 있게 해 준다. 어플리케이션을 배포하거나 docker image로 만들 때는 JAR 패키지로 패키징하여 실행하는 방법이 유용하게 사용된다. 

실행 가능한 Jar 파일(Executable Jar) 만들고 실행하기
Jar 파일 생성하기

target 하위를 비운다 (maven 빌드로 생성된 모든 산출물을 삭제한다)

 mvn clean
실행 가능한 JAR 파일 하나가 생성된다.

 mvn package
target 폴더 하위에 jar 파일이 만들어져 있다. 

command line으로 Jar 실행하기

java 명령어로 jar 파일을 함 실행해보자.

java -jar target/{jar파일 이름}.jar

놀랍게도(?) jar 파일 하나만 있어도 앱이 돌아간다. mvn package 만든 jar 파일 안에, 내가 작성한 Java 클래스와 리소스, 라이브러리들의 jar 파일이 모두 다 들어가 있다. spring-boot-loader 모듈은 스프링 부트의 실행 가능한 jar 파일을 만들어준다.  maven이나 gradle 플러그인을 사용하고 있다면 실행가능한 jar가 자동으로 생성된다.

 

실행 가능한 jar 포맷이 쓰이는 이유

자바는 nested jar를 로드할 수 있는 표준 방법을 제공하지 않는다. 이는 필요한 모든 의존성을 포함하는 어플리케이션(self-contained application)을 배포해야 할 때 문제가 된다. 이를 해결하기 위해서, 과거엔 jar 패키지들의 모든 클래스를 하나로 압축하는 uber jar를 사용했다. 다만 uber jar는 어떤 라이브러리를 쓰는지 알기 어렵고, 같은 이름을 가진 파일이 있으면 문제가 될 수 있다.

 

이런 단점을 해결하기 위해 스프링 부트는 다른 접근 방식을 사용한다.

The Executable Jar File Structure
example.jar
 |
 +-META-INF
 |  +-MANIFEST.MF
 +-org
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes>
 +-BOOT-INF
    +-classes
    |  +-mycompany
    |     +-project
    |        +-YourClasses.class
    +-lib
       +-dependency1.jar
       +-dependency2.jar
org.springframework.boot.loader.jar.JarFile : nested jar들을 읽을 수 있는 파일이다. 
org.springframework.boot.loader.jar.Launcher : Launcher는 실행 가능한 jar의 실행 지점(Main-Class)으로 사용되는 특수한 파일이다. 적절한 URLClassLoader를 설정하고 최종적으로 main() 메서드를 호출한다.
executable jar file의 메타 정보를 담고 있는 MANIFEST.MF를 확인해보면 이에 대한 정보가 담겨있다.

Main-Class: org.springframework.boot.loader.JarLauncher
Start-Class: com.mycompany.project.MyApplication
실행 가능한 jar는 스프링 부트를 독립적으로 실행 가능한 어플리케이션으로 만들어준다. 이는 스프링 부트의 주요 목표중 하나다.
