---
title: "spring security 도전기"     
date: 2020-06-16 13:20:28 -0400
categories: 공부
---
> springboot, jpa, thymeleaf 로 게시판 프로젝트 중에 로그인/로그아웃 + 권한제어 기능을 추가하려고 spring security 를
추가했습니다.

## Spring Security 란?
 - 스프링 기반의 어플리케이션의 인증과 권한을 담당하는 프레임워크
 
## 인증, 인가

 - 로그인을 했을 때 DB 에 저장된 정보와 회원이 친 loginId, password 가 일치 했을 때 (현재 아이디의 정보가 누구인지 확인) = 인증 (Authentication)
 - 관리자로써 권한을 갖고 로그인을 했을 때 일반 회원은 볼 수 없는 비밀 댓글의 내용 혹은 회원 정보등을 볼 수 있을 때 (권한 검사) = 인가 (Authorization)
 
## Spring Security 구조

![springsecurity](https://user-images.githubusercontent.com/45488643/84763801-bc61b100-b007-11ea-8951-5ef428e48cca.png)


![다운로드 (1)](https://user-images.githubusercontent.com/45488643/84764146-38f48f80-b008-11ea-8f84-711e12a509c1.png)


일단 사용하려면 dependency 부터 걸어줍시다.

**pom.xml**
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

그런 후에 spring security config 를 만들어서 셋팅 해줍시다.
예를 들어 로그인한 회원만 글 작성 혹은 글을 열람할 수 있고, 로그인하지 못한 게스트는 기능 제한을 해주는 세팅을 해줍시다.

**SpringSecurityConfig**
```java
@Configuration
@EnableWebSecurity
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .mvcMatchers("/users/**").hasRole("ADMIN")
                .mvcMatchers("/posts/**").hasRole("MEMBER")
                .anyRequest().permitAll()
                .and()
                .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
                .logout()
                .logoutSuccessUrl("/")
                .and()
                .csrf()
                .disable();
    }
}
```

|메소드|설명|
|------|---|
|authorizeRequests()|HttpServletRequest 를 이용해서 인증 처리
|mvcMatchers()|mvcMatcher(String mvcPatter) - 제공된 Spring MVC 패턴과 일치하는 경우에만 HttpSecurity를 호출하도록 설정할 수 있습니다.
|antMatchers()|AntMatcher(String AntPatter) - 제공된 ant 패턴과 일치하는 경우에만 HttpSecurity를 호출하도록 설정할 수 있습니다
|hasRole()|특정 권한 있는 사람만 접근 가능합니다.
|permitAll()| 모든 사용자가 접근 가능하다는 것을 의미합니다.
|anyRequest().permitAll()|그 외 모든 요청은 접근 가능
|csrf()|Token 정보를 Header 정보에 포함하여 서버 요청을 시도하는 것 (쉽게 말해 보안 강화)

spring security config 설정이 다 끝나면 service 에서 처리해줘야합니다. UserService 가 있지만 역할 분담을 위해 UserAuthService 를 따로 만들었습니다.

**UserAuthService**
```java
public class UserAuthService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username).orElseThrow(() -> new UsernameNotFoundException("존재하지않는 회원입니다"));
        List<GrantedAuthority> authorities = new ArrayList<>();
        GrantedAuthority authority = new SimpleGrantedAuthority(user.getRole());
        authorities.add(authority);
        return new org.springframework.security.core.userdetails.User(user.getUsername(), user.getPassword(), authorities);
    }
}
```
User 가 두개가 있습니다. **제가 직접만든 User Entity**, 그리고 **spring security 에서 지원하는 User**

|코드|설명|
|------|---|
|findByUserName|로그인 처리를 하기 전 username (=loginId) 를 가져옵니다.
|UserDetails|Spring Security에서 사용자의 정보를 담는 인터페이스는 UserDetails 인터페이스우리가 이 인터페이스를 구현하게 되면 Spring Security에서 구현한 클래스를 사용자 정보로 인식하고 인증 작업을 합니다
|UserDetailsService|DB에서 유저 정보를 직접 가져오는 인터페이스를 구현
|GrantedAuthority|권한 있는 사용자인지 판단해주는 타입
                    
## 주의사항

 - 회원 아이디를 받을 때 entity 에서 username 으로 받아줘야한다. (spring security 규약을 지키기위함)
 - 회원 비밀번호를 받을 때 entity 에서 password 로 받아줘야한다. (//)
 
-> 규약을 지키지않으면 403 에러와 로그인처리가 전혀 되지않는다. (방법을 알고계신 분 eojin312@naver.com 으로 알려주세요 ㅠㅠ)

 - role 을 같이 넘겨주지않으면 일반적으론 403 에러가 발생한다.

    



