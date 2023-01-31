# 6강 - AOP

## AOP가 필요한 상황
* 모든 메소드의 호출 시간을 측정하고 싶다면?
* 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)
* 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

![img.png](img.png)

### MemberService 회원 조회 시간 측정 추가
`src/service/MemberService.java`
```java
@Transactional
public class MemberService {
    // ...
    /**
     * 회원 가입
     */
    public Long join(Member member) {
        long start = System.currentTimeMillis();

        try {
            validateDuplicateMember(member);
            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join " + timeMs + "ms");
        }
    }

    /**
     * 전체 회원 조회
     */
    public List<Member> findMembers() {
        long start = System.currentTimeMillis();

        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("join " + timeMs + "ms");
        }
    }
    // ...
}
```

### 문제
* 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.
* 시간을 측정하는 로직은 공통 관심 사항이다.
* 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
* 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
* 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해 한다.

## AOP 적용
* AOP: Aspect Oriented Programming
* 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리

![img_1.png](img_1.png)

### 시간 측정 AOP 등록
`src/aop/TimeTraceAop.java`
```java
@Aspect
@Component
public class TimeTraceAop {
    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        System.out.println("START: " + joinPoint.toString());

        try {
            return joinPoint.proceed();
        } finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;

            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```

### 해결
* 회원가입, 회원 조회등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다.
* 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
* 핵심 관심 사항을 깔끔하게 유지할 수 있다.
* 변경이 필요하면 이 로직만 변경하면 된다.
* 원하는 적용 대상을 선택할 수 있다.

## 스프링의 AOP 동작 방식 설명

### AOP 적용 전 의존관계
![img_2.png](img_2.png)

### AOP 적용 후 의존관계
![img_3.png](img_3.png)

### AOP 적용 전 전체 그림
![img_4.png](img_4.png)

### AOP 적용 후 전체 그림
![img_5.png](img_5.png)