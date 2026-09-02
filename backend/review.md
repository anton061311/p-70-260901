## 2026.09.01 (1-14)

# 2강, 회원 도메인 도입하고 샘플 데이터 5명 생성
인증 -> 넌 누구? 신원확인
인가 -> 너가 해도 되는 거야? 권한 확인
mkdir domain/member에서 repo, entity, service 만들고 baseinit data 추가

# 3강, 글과 댓글에 작성자 등록. 작성자는 임의로 user1로 설정. self.work2() 호출 누락된 것 추가
Option형 findByYsername을 맴버 레포에 구현
!-Post가 N 쪽이니까 ManyToOne을 작성해야 한다.
  ManyToOne은 필수이다. 그리고 왜래키를 가지고 있는 쪽이 Many이다.
  fetch 를 LAZY로 설정하면 포스트만 가져오고 member가 필요한 시점, 그 시점에 가져온다. 
  안 가져왔다면 시작부터 member도 가져왓을 거임. 

# 4강, postDto에 작성자 정보 authorId와 authorName 추가. 글 작성 시 author 연결 누락된 부분 수정
!-개발할 때 위험한 방법은 나중에 사용하지 않을까 싶어서 개발하는 것이 위험한 습관이다

# 5강 - DTO에는 꼭 필요한 데이터만 담는다. 최소공개원칙
필요한 거 당므셔라 이 얘기입니다.

# 6강, CommentDto에도 작성자 정보 추가, postId도 같이 추가
넘어가시구요

# 7강, Dto의 스펙이 바뀌면 원래 테스트 케이스부터 고쳐야 하지만 일단은 Dto 수정하고 테스트에 반영
7강은 방금 위(4강)에서 같이 테스트 케이스 작성한 겁니다.

# 8강, username 파라미터를 입력 받아 작성자가 누구인지 스프링부트가 알게 함

# 9-10강,비밀번호까지 입력받아서 사칭을 못하게 막고, 예외를 상세하게 처리하는 것은 좋지만 너무 방대하다. 우리 서비스에 맞는 ServiceException 도입
MisMatchHandlerException구현했다. 그럼에도 예외 케이스는 매우 많다. 로그인을 싪패하는 이유만 해도 
아이디 오류, 비밀번호 오류, 블랙리시트, 인증 대기 등이 있다. 정말 정성껏 하고 싶다면 이 예외들을 만드는 방법이 있다. 
원리 이를 클래스를 하나씩 만드는 게 정석이긴 하다. 로그인만 해도 많은데 예외에 대한 클래스를 하나하나 만들면 귀찮다. 그래서 이렇게 할 수 있다 라는
걸 알아만 두고, 우리는 프로젝트의 규모가 너무 크지 않고, 어떤 예외 케이스가 핵심적이고 얘네들을 섬세하게 다룰 필요가 없는데 클래스를 하나하나 
구현하는 것은 오버엔지니어링이다. 따라서 서비스/도메인 예외로 퉁치긴 한다. 하나로 통일해 구체적인 응답 코드와 메시지로 처리한다. 이 장점은
일단 편하다. 내가 개발을 할 때 편하다. 일단 이거로 가고 꼭 필요한 부분만 위처럼 도입하는 것이 좋다. 그래서 우리는 일단 방금 구현한 예외 삭제함.
그래서 모든 예외를 공유할 걸 global/exception/ServiceException에 만들 것이다. 이 ServiceException은 우리가 만든 예외고,
우리가 만든 예외를 사용할 수 있다.

# 11강, 보고서 양식 문제는 해결. 인증 데이터가 문제. 일단 문제점 인지
서버 -> REST API -> 무상태성(stateless) 추구
클라이언트에 인즈 ㅇ정보 저장 -> 불안 0> 서버에 저장 -> 무상태성 위반 -> 프론트 -> JTW

프론트에서 인증 데이터로 ID / PW를 넘김
-> ID-PW:매우 민감함 데이터

인증데이터를 좀 더 무의미하고 무작위한 값으로 하자 -> apiKey라고 명명하겠습니다.
apiKey는 어떤 유의미한 값이 있어서는 안 되고 무작위 해야 한다.

# 12강,  회원에게 apiKey 추가, username과 apiKey에 유니크 설정 추가
@Column(unique = true)

# 13강, 이제 글 작성시 username/password가 아니라 apiKey로 인증

# 14강, TDD 방식으로 ApiV1MemberController 구현. 테스트 유저 데이터는 {"newUser", "1234", "새유저"}

* 오늘까지 체크 포인트는 아이디 비밀번호 대신 apikey를 만드는 방법이 있다.
---

## 2026.09.02 (14-?)
# 15강, 회원 가입시 중복된 아이디 처리, 테스트 케이스 먼저 만들고 구현
* commit message - test: check for already exist username
* commit message - feat(member): implement ApiV1MemberController 

# 16강, username 중복 처리 로직을 memberService로 옮겨서 재사용성을 높임
* commit message - 


# 17강, 로그인 기능 구현
* commit message -
 인증 != 로그인입니다.
 인증이라는 건 어떤 수단을 가지고 이 api를 사용하는 살마에게 인증을 요구하는 것이다. 너임을 증명해라. 
 그리고 그림을 그려줌.
 제 API키인데 다른 유저에게 사용하도록 보내
 인증테이터가 필요하다면 최초로 한 번은 인증 데이터가 필요하다고 보내야합니다. 그래서 로그인이라는 것은 인증 데이터를 달라고 하는 행위이지 인증은 아닙니다. 
 이 인증 데이터를 확보를 하는 행위로 확보된 데이터를 통해 인증을 할 겁니다 앞으로.
 dto를 만들 땐 외부까지 가실 걸 생각해 보고 프론트로 갈 건 위험해 질 수도 있기 떄문에 민감한 정보는 프론트에 가지 않게 해야함. 그래서 apikey를 dto에 넣지 않겠다.
 따라서 dto와 apikey를 따로 전달할 것이다.
선택이 갈린다 여기서. 조인이랑 로그인 둘 ㄷ ㅏ포스트를 ㅡㅆ게 되면서 충돌이 난다. 그래서 ㅇ뤼는 이걸 해결해야 한다. 
 
** 코드 복붙**

1. 컨트롤러 분리
- 회원가입: 단순 회원 기능
- 로그인: 회원 인증 기능

2. 동사를 사용 
- POST/api/v1/members/join: 회원가입
- POST/api/v1/members/login: 로그인
-> RESTFUL하지 않다. 동사를 사용하지 않다는 게 규칙이었다.
정말 점격하게 규칙을 따르고 싶다면 1번을 따르는 게 맞다.
웬만해서 RESTFUL하게 가는 게 맞지만 백과 프론트의 우리(같은 사람)의 통제 하에 있다.

개발 방법론 ex TEST API, 디자인 패턴 등을 너무 맹신하지 않는 것이 좋다. 
우리의 목표가 있다면 그 목표에 맞는 방법론을 사용하는 게 좋다. 예컨데 RESTFUL을 사용하지 않는 게 좋다. 왜 이렇게 했냐고 했을 때 이유를 말해주는 것이 좋다.rstful하게

구현은 같이 해보는 것이 좋을 것 같아서 해봄

여기를 거의 1시간을 설명했음

# 18강 1부, 콜렉션에 baseURL 변수 지정하고 각 요청에 활용
# 18강 2부, POSTMAN에서 콜렉션 변수 apiKey 만들어서 사용
* commit message -

*  잠깐 이전 설명 복습
로그인을 하고 나면 apikey만 가지고 클라이언트와 서버가 소통할 수 있다
그리고 서버에만 저장하는 순간 무상태성을 위반하는데 프론트에 저장하면 위험하긴 하다.
그래서 아이디 비번 보다는 apikey를 프론트에 저장한다. -> 뭔소리일까?

# 19강, 1부, apiKey를 header에 Authorization으로 보내기
# 19강, 2부, Authorization 헤더를 백엔드에서 처리
* commit message - refactor(member): handle API key from Authroization header
 현재 인증데이터(apiKey)가 url 파라미터로 넘어가고 있음.
 갑자기 body로 apikey를 넘김. (Postman에서)
 request param이아니라 postwritereqbody에서 string apikey로 바꾸는데? 
 아 바디에 포함시키는 거구나
 url에 포함: 위험
 body에 포함: 근데 BODY에 포함시키면 안 좋은 점도 있따. GET 요청은 requestBody가 없다. 그래서 하나의 방법이긴 하지만 제약이
있기 때문에 보통 body에 포함하진 않는다.
 header에 포함: 모든 요청에 헤더는 필수 존재.
 따라서 인증데이터는 header에 넣는 것이 좋다.
 Postman의 header탭에 들어가서 apikey를 넣음![img_1.png](img_1.png)

# 20강, 포스트맨에서 최상위 부모에게 auth type을 설정하면 자식들이 설정을 물려 받음. 이제 Authorization 헤더를 직접 넣지 않아도 된다.
* 포스트맨만 사용
이제 포스트맨에서 처리해야 하는 부분은 끝났습니다.

# 21강, 글 수정시에도 인증정보를 전달, 글 수정시 권한체크(인가)까지 진행
* test(member): set sample API keys to match usernames
인증 -> 누구인지 증명
인가 -> 누군지는 알겠고 권한이 있나 체크
 
member에 생성자 추가


# 22강, 글 삭제 인증과 권한 처리
feat(post): add authentication and authorization for post deletion
* 구체적으로는 Authorization: Bearer ... 헤더에서 API Key를 추출하고, 유효하지 않은 키는 401, 작성자가 아닌 사용자의 삭제 요청은 403으로 차단했습니다. 기존 글 작성/수정 요청의 Authorization 헤더에도 @NotBlank 검증을 추가했고, 관련 테스트에도 인증 헤더를 추가했습니다.

# 23강, 글 등록 테스트케이스에도 인증과 권한 내용 추가

# 24강,

# 25강,

# 26강,

# 27강,

# 28강,

# 29강, 

## 2026.09.03 (?-?)
