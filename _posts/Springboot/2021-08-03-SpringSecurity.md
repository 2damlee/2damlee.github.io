---
title: "Spring security란"
categories:
  - Springboot
layout: archive
sidebar_main: true
author_profile: true
tag:
  - Springboot
toc: true
---

<br>

<br>

### Spring security란

스프링 기반의 어플리케이션 보안(인증과 권한)을 담당하는 스프링 하위 프레임워크. 자체적으로 세션을 체크하고 redirect하는 대신 스프링 시큐리티는 보안과 관련되어 이를 지원해준다.  Spring Security는 인증과 권한의 부분을 Filter 흐름(*Dispatcher Servlet으로 가기 전에 적용, 가장 먼저 url 요청 받음)에 따라 처리한다. 

<img src="/assets/images/project/blogpost-spring-security-architecture.png">



<br>

<br>

##### 용어 정리

- Principal: 보호된 대상에 접근하는 유저
- Authenticate: 현재 유저를 확인, 어플리케이션 작업을 수행할 수 있는 유저인지를 확인, 증명하는 인증 과정
- Authorize: 현재 유저가 어떤 서비스와 페이지에 접근 가능한지 권한을 검사
- Authorization: 인증된 유저가 어플리케이션 수행에 허가되었는지 결정(인가)

<br>

##### 인증 방식

- 인증(Authentication)과 인가(Authorization)

  Spring security는 인증과 인가를 위해 Principal을 아이디로, Credential을 비밀번호로 사용하는 <u>Credential 기반의 인증 방식</u> 

인증 방식은 세션-쿠키 방식으로 인증된다. 

1. 유저가 로그인(http request)을 시도
2. AuthenticationFilter를 통해 user DB확인
3. DB에 존재하면 UserDetail로 꺼내 유저의 session을 생성
4. spring security의 인메모리 세션 저장소 SecurityContextHolder에 저장
5. 유저에게 session id와 함께 응답
6. 최초 요청 이후에는 요청쿠키에서 JSESSIONID를 확인 후 유효하면 인증을 주는 방식

<br>

#### Spring Security 모듈

- SecurityContextHolder

  현재 보안 컨텍스트에 대한 세부정보가 저장 

- SecurityContext

  Authentication을 보관하는 역할. SecurityContext를 통해 Authentication 객체를 꺼내올 수 있다.

- Authentication

  현재 접근하는 주체의 정보와 권한을 담당하는 인터페이스. Authentication객체는 Security Context에 저장, SecurityContextHolder를 통해 SecurityContext에 접근 가능

  ```java
  public interface Authentication extends Principal, Serializable { 	// 현재 사용자의 권한 목록을 가져옴 
    Collection<? extends GrantedAuthority> getAuthorities(); 
    
    // credentials(비밀번호)을 가져옴 
    Object getCredentials(); Object getDetails(); 
    
    // Principal 객체를 가져옴 
    Object getPrincipal(); 
    
    // 인증 여부를 가져옴 
    boolean isAuthenticated(); 
    
    // 인증 여부를 설정
    void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException; }
  
  ```

  

- UsernamePasswordAuthenticationToken

  Authentication을 implements한 AbstractAuthenticationTokem의 하위클래스. User의 id가 principal, Password가 Credential역할을 함. 첫번째 생성자는 인증 전의 객체, 두번째 생성자는 인증이 완료된 객체를 생성한다.

  ```java
  public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken { 
    // 주로 사용자의 ID
    private final Object principal; 
    
    // 주로 사용자의 PW
    private Object credentials; 
    
    // 인증 완료 전   객체 생성 
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) { 
      super(null); 
      this.principal = principal; 
      this.credentials = credentials; 
      setAuthenticated(false);
    } 
    
    // 인증 완료 후의 객체 생성 
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials, Collection<? extends GrantedAuthority> authorities) {
      super(authorities); 
      this.principal = principal; 
      this.credentials = credentials; 
      super.setAuthenticated(true); 
                                                                                                                                          	} 
  }
  
  ```

- AuthenticationProvider

  실제 인증에 대한 부분 처리. 인증 전 Authentication객체를 받아 인증이 완료된 객체를 반환. 

  Authentication 인터페이스를 구현해 Custom한 AuthenticationProvider를 작성 후 AuthenticationManager에 등록해서 사용

  ```java
  public interface AuthenticationProvider { 
    // 인증 전의 Authenticaion 객체를 받아서 인증된 Authentication 객체를 반환 
    Authentication authenticate(Authentication var1) throws AuthenticationException; 
    
    boolean supports(Class<?> var1);
  }
  ```

- AuthenticationManager

  인증에 대한 부분에 대한 처리. 인증 성공시 두번째 생성자를 사용해 인증이 성공한 객체(=true)를 생성해 Security Context에 저장. 그리고 인증 상태를 유지하기 위해 session에 보관, 실패시 AuthenticationException 발생

  ```java
  public interface AuthenticationManager { 
    Authentication authenticate(Authentication authentication) throws AuthenticationException;
  }
  ```

  AuthenticationManager를 상속받은 ProviderManager은 인증 과정에 대한 로직을 갖고 있는 AuthenticationProvider를 갖고 있고, 반복문을 통해 provider를 조회하며 authenticate 처리를 한다.

  ```java
  public class ProviderManager implements AuthenticationManager, MessageSourceAware, InitializingBean { 
    
    public List<AuthenticationProvider> getProviders() {
      return providers; 
    } 
    
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
      
      Class<? extends Authentication> toTest = authentication.getClass();
      AuthenticationException lastException = null;
      Authentication result = null;
      boolean debug = logger.isDebugEnabled();
      
      //for문으로 모든 provider를 순회 
      for (AuthenticationProvider provider : getProviders()) { 
        .... 
          try { 
            result = provider.authenticate(authentication);
            if (result != null) {
              copyDetails(authentication, result);
              break;
            }
          }
        catch (AccountStatusException e) {
          prepareException(e, authentication);
          // SEC-546: Avoid polling additional providers if auth failure is due to 
          // invalid account status 
          throw e; 
        }
        .... 
      } 
      throw lastException; 
    } 
  }
  ```

  * 직접 구현한 CustomAuthenticationProvider를 등록하는 방법은 WebSecurityConfigurerAdapter를 상속받은 Config를 통해 하면 된다. 

- UserDetail

  인증에 성공시 생성되는 객체. Authentication객체를 구현한 UsernamePasswordAuthenticationToken을 생성하기 위해 사용. UserDetails의 인터페이스는 직접 개발한 UserVO모델에 UserDetail을 implements하여 처리할 수 있다. 

  ````java
  public interface UserDetails extends Serializable {
    Collection<? extends GrantedAuthority> getAuthorities();
    String getPassword();
    String getUsername();
    boolean isAccountNonExpired();
    boolean isAccountNonLocked();
    boolean isCredentialsNonExpired();
    boolean isEnabled();
  }
  ````

- UserDetailService

  UserDetails객체를 반환하는 메소드를 가진 인터페이스. 이를 구현한 클래스 내부에 UserRepository를 주입받아 DB와 연결, 처리한다. 

  ```java
  public interface UserDetailsService {
    UserDetails loadUserByUsername(String var1) throws UsernameNotFoundException;
  }
  ```

- PasswordEncoding

  AuthenticationManagerBuilder.userDetailsService().passwordEncoder() 를 통해 패스워드 암호화에 사용될 PasswordEncoder 구현체를 지정이 가능하다. 

  (*암호화를 통하지 않을 경우 DB에 개인 정보가 그대로 남기 때문에 보안상 필히 사용하는 것이 좋다.)

  ```java
  @Override 
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    // TODO Auto-generated method stub
    		auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
  } 
  @Bean 
  public PasswordEncoder passwordEncoder(){
    return new BCryptPasswordEncoder();
  }
  ```

* GrantedAuthority

  현재 사용자가 갖고 있는 권한. ROLE_ADMIN나 ROLE_USER와 같이 ROLE_*의 형태로 사용된다. GrantedAuthority 객체는 UserDetailService에 의해 불러올 수 있으며 특정 자원에 대한 권한여부를 검사하여 접근 허용 여부를 결정한다. 



참고 <a href="https://mangkyu.tistory.com/76">📎</a>

