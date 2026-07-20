# OAuth 2.0 - 구글/카카오 로그인의 작동 원리
"구글로 로그인" 버튼 뒤에 무슨 일이 벌어지는지 알면 구현이 쉬워진다. | 2026-07-20

### 개념
OAuth 2.0은 사용자가 비밀번호를 우리 서비스에 주지 않고도 구글, 카카오 등 외부 서비스의 계정으로 로그인할 수 있게 해주는 프로토콜이다.</br>
우리 서버는 구글한테 "이 사람이 구글 계정을 가진 게 맞냐"라고 확인만 받으면 된다.

### 등장인물
`Resource Owner` - 사용자. 구글 계정을 가진 사람을 지칭.
`Client` - 우리가 만드는 서비스 (백엔드 서버).
`Authorization Server` - 구글, 카카오 등 인증을 담당하는 서버.
`Resourece Server` - 구글 이메일, 프로필 등 실제 데이터 보유 서버.

### Authorization Code Flow
1. 사용자가 "구글로 로그인" 클릭 → 구글 로그인 페이지로 리다이렉트
2. 사용자가 구글에서 로그인 + 권한 동의
3. 구글이 우리 서버 Redirect URI로 Authorization Code 전달
4. 우리 서버가 코드 + Client Secret로 구글에 Access Token 요청
5. Access Token으로 구글 API 호출 → 사용자 이메일 및 이름 조회
6. 우리 DB에 유저 저장 → 우리 서비스 JWT 발급


> *Client Secret은 반드시 서버에서만 실행* - Authorization Code를 Access Token으로 교환하는 과정은 서버 사이드에서만 해야 한다.</br>
프론트엔드에서 Client Secret을 쓰면 누구나 볼 수 있어서 치명적인 보안 취약점이 된다.</br>

> *JWT와 연결* - OAuth로 구글에서 사용자 정보를 받아온 뒤, 우리 서비스의 JWT를 새로 발급해서 클라이언트에서 준다.<br/>
OAuth는 "신원 확인용", 이후 우리 API 인증은 우리 JWT로 하는 게 일반적인 패턴이다.
