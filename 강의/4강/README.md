# 4강 - 회원 관리 예제 - 웹 MVC 개발

## 회원 웹 기능 - 홈 화면 추가
* 홈 컨트롤러 추가

`src/controller/HomeController.java`
```java
@Controller
public class HomeController {
    @GetMapping("/")
    public String home() {
        return "home";
    }
}
```

* 회원 관리용 홈

`src/resources/templates/home.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Home</title>
</head>
<body>
  <div class="container">
    <h1> Hello Spring </h1>
    <p>회원 기능</p>
    <p>
      <a href="/members/new">회원 가입</a>
      <a href="/members">회원 목록</a>
    </p>
  </div>
</body>
</html>
```

## 회원 웹 기능 - 등록
* 회원 등록 폼 컨트롤러

`src/controller/MemberController.java`
```java
@Controller
public class MemberController {
    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }

    @GetMapping("/members/new")
    public String createForm() {
        return "members/createMemberForm";
    }

    @PostMapping("members/new")
    public String create(MemberForm form) {
        Member member = new Member();
        member.setName(form.getName());

        memberService.join(member);

        return "redirect:/";
    }
}
```

* 회원 등록 폼 Class

`src/controller/MemberForm.java`
```java
public class MemberForm {
    private String name;

    public String getName() {
        return this.name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

* 회원 등록 폼 HTML

`src/resources/templates/members/createMemberForm.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Create Member</title>
</head>
<body>
<div class="container">
  <form action="/members/new" method="post">
    <div class="form-group">
      <label for="name">이름</label>
      <input type="text" id="name" name="name" placeholder="이름을 입력하세요"/>
      <button type="submit">등록</button>
    </div>
  </form>
</div>
</body>
</html>
```

## 회원 웹 기능 - 조회
* 회원 컨트롤러에서 조회 기능

`src/controller/MemberController.java`
```java
@Controller
public class MemberController {
    // ...
    @GetMapping("members")
    public String list(Model model) {
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";
    }
}
```

* 회원 리스트 HTML

`src/resources/templates/members/memberList.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="UTF-8">
  <title>Create Member</title>
</head>
<body>
<div class="container">
  <div>
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>이름</th>
        </tr>
      </thead>
      <tbody>
        <tr th:each="member: ${members}">
          <td th:text="${member.id}"></td>
          <td th:text="${member.name}"></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
</body>
</html>
```