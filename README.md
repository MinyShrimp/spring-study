# 스프링 입문 : 스프링 부트
* 링크 : [인프런 스프링 입문 - 김영한](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)

# 0강 - 프로젝트 환경설정
## 폴더 구조
* .idea : IntelliJ 설정 폴더
* gradle : Gradle 
* src : 소스 코드 폴더
  * main : 실제 실행되는 코드 폴더
    * java : Java 소스 코드
    * resources : Java 소스 코드 외
  * test : 테스트 코드 폴더
* build.gradle : Spring 관련 설정 파일
* setting.gradle

## 라이브러리 살펴보기
* spring-boot-starter-web
  * spring-boot-starter-tomcat: 톰캣 (웹서버)
  * spring-webmvc: 스프링 웹 mvc
* spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진 (View)
* spring-boot-starter (공통): 스프링 부트 + 스프링 코어 + 로깅
  * spring-boot
    * spring-core
  * spring-boot-starter-logging
    * logback, slf4j
* spring-boot-starter-test
  * junit: 테스트 프레임워크
  * mockito: 목 라이브러리
  * assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  * spring-test: 스프링 통합 테스트 지원
  
## View 환경설정

### 정적 페이지 ( index.html )
```html
<!-- src/resources/static/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome</title>
</head>
<body>
    Hello
    <a href="/hello">hello</a>
</body>
</html>
```

### thymeleaf 템플릿 엔진
* [thymeleaf 공식 사이트](https://www.thymeleaf.org/)
* [스프링 공식 튜토리얼](https://spring.io/guides/gs/serving-web-content/)

* 컨트롤러에 리턴 값으로 문자를 반환하면 뷰 리졸버(ViewResolver)가 뷰(View)를 찾아서 처리한다.
  * 스프링 부트 템플릿엔진 기본 viewName 매핑
  * `'resources:templates/' + { ViewName } + '.html'`

## 빌드하고 실행
1. 콘솔 실행
2. `./gradlew build`
3. `cd build/libs`
4. `java -jar hello-spring-0.0.1-SNAPSHOT.jar`
5. 실행 확인