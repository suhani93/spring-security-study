# Spring Security 란?

> Web Application의 인증과 인가 기반의 보안 기능 쉽게 지원해주는 Spring 프레임워크
>
> Filter 기반으로 동작한다.



# 인증 (Authenticate)

> 현재 유저가 누구인지 확인하는것이다.
>
> 인증 실패 시 HTTP 코드는 401 Unauthorized를 리턴한다.



# 인가 (Authorize)

> 현재 유저가 어떤 서비스에 접근할 수 있는지 권한을 검사하는 것이다.
>
> 권한이 없는 페이지에 접근시 403 Forbidden 를 리턴한다.



# 접근 주체 (Principal)

> 보호된 대상에 접근하는 유저이다.



# Spring Security 의 인증 아키텍처




1. 요청을 보낸다

   - 브자우저에서 사용자가 아이디와 비밀번호를 입력하고 요청을 보낸다.

2. 인증용 객체(Token)를 만든다.

   -  AuthenticationFilter가 요청을 받아서 UsernamePasswordAuthenticationToken토큰을 생성한다.

3. 인증을 담당한 Authentication Manager에게 Token 값을 넘겨준다.

   - Authentication Manager에서 Provider를 통해 각 Provider의 supports 메소드로 확인한다.

4. Token을 처리할 수 있는 Authentication Provider 선택한다.

5. 인증을 시작 한다.

   - UserDetailsService의 loadUserByUsername 메소드를 통해 인증을 한다.

6. 유저 정보를 기반으로 사용자의 정보를 불러온다.

   



# SecurityFilterChain

>Spring Security의 filter는 서블릿 필터를 기반으로 작동한다.





- SecurityContextPersistenceFilter - 요청(request)전에, SecurityContextRepository에서 받아온 정보를 SecurityContextHolder에 주입합니다.
- LogoutFilter - 주체(Principal)의 로그아웃을 진행합니다. 주체는 보통 유저를 말합니다.
- UsernamePasswordAuthenticationFilter - (로그인) 인증 과정을 진행합니다.
- DefaultLoginPageGeneratingFilter - 사용자가 별도의 로그인 페이지를 구현하지 않은 경우, 스프링에서 기본적으로 설정한 로그인 페이지를 처리합니다.
- BasicAuthenticationFilter - HTTP 요청의 (BASIC)인증 헤더를 처리하여 결과를 SecurityContextHolder에 저장합니다.
- RememberMeAuthenticationFilter - SecurityContext에 인증(Authentication) 객체가 있는지 확인하고RememberMeServices를 구현한 객체의 요청이 있을 경우, Remember-Me(ex 사용자가 바로 로그인을 하기 위해서 저장 한 아이디와 패스워드)를 인증 토큰으로 컨텍스트에 주입합니다.
- AnonymousAuthenticationFilter - SecurityContextHolder에 인증(Authentication) 객체가 있는지 확인하고, 필요한 경우 Authentication 객체를 주입합니다.
- SessionManagementFilter - 요청이 시작된 이 후 인증된 사용자 인지 확인하고, 인증된 사용자일 경우SessionAuthenticationStrategy를 호출하여 세션 고정 보호 메커니즘을 활성화하거나 여러 동시 로그인을 확인하는 것과 같은 세션 관련 활동을 수행합니다.
- ExceptionTranslationFilter - 필터 체인 내에서 발생(Throw)되는 모든 예외(AccessDeniedException, AuthenticationException)를 처리합니다.
- FilterSecurityInterceptor - HTTP 리소스의 보안 처리를 수행합니다.



https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-architecture

