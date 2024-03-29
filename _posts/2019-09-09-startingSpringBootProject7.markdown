---
layout: post
title: "SpringBoot 프로젝트 시작하기 7 - 스프링 시큐리티 JWT"
date: 2019-09-09
categories: SpringBoot
tags: SpringSecurity JWT
comments: true
---
<div style="display:none;">
어떻게 글을 짤까, Spring Security에 심화적인 부분 왜 JWT가 나왔는가에 대해 Oauth까지 설명
웹어플 로그인 요청 시, 프론트 엔드와 백엔드로 나뉘었을 때 문제점 기술
</div>
<h3>세션-쿠키</h3>
<br>
&nbsp;여러분들은 스프링 시큐리티를 읽으면서 이런 의문이 들었을 수도 있습니다. 무엇을 근거로 이 아이디가 접속했다고 할 수 있을까? 그에 웹서버는 <b><a href="https://jeong-pro.tistory.com/80">세션-쿠키</a></b> 방식으로 사용하기에 알 수 있다라고 답할 수 있습니다. 좀 더 정확히 말하자면 서버는 클라이언트가 처음 접근할 때 세션ID를 발급해주고 클라이언트에서 쿠키를 사용해 저장합니다. 이 다음 클라이언트가 서버에 접근할 때마다 쿠키를 이용하여 세션ID값을 전달함으로써 서버가 이 전에 접근했던 클라이언트임을 알 수 있는 것입니다. 로그인 후에 로그인 정보를 서버와 클라이언트에 세션-쿠키방식으로 저장함으로써 로그인을 유지 할 수 있는 것입니다. 그래서 이 프로젝트에 문제가 생깁니다.
<br><br>
![SessionCookie](/files/security/SessionCookie.png)
<br><br>
<hr class="divider">
<h3>서버 분리 문제점</h3>
<br>
&nbsp;세션-쿠키방식은 널리 쓰는 방식이며 전혀 문제가 없어보입니다. 하지만 왜 문제가 있다고 말하는 것일까 여러분들은 궁금할 것입니다. 
이 프로젝트는 백엔드 서버와 프론트엔드 서버로 서버를 나누기 때문에 문제가 생깁니다. 
프론트 서버에서 로그인 하게 되면 그 로그인의 정보는 클라이언트의 브라우저와 프론트 서버에 저장됩니다. 
그럼 백엔드 서버는 로그인 했다는 것을 알 수 있을까? 당연히 알 수 없습니다. 
그럼 어떻게 해야 이 문제를 해결 할 수 있을까? 그 해답은 바로 JWT에 있습니다.
<br><br>
<hr class="divider">
<h3>JWT란</h3>
<br>
&nbsp;<b><a href="https://velopert.com/2389">JWT(JSON Web Token)</a></b>을 간단하게 소개하자면 웹표준 (RFC 7519) 으로서 JSON 객체를 사용하여 가볍고 자가수용적인 방식으로 정보를 안전성 있게 전달할 수 있습니다. 가장 많이 쓰이는 방식을 바로 회원 인증인데 JWT에 자기수용적인 방식으로 회원의 정보를 가지고 있어 클라이언트가 요청 시 서버는 단지 전달받은 JWT가 유효한지 인증됐는지를 검증하기만 하면됩니다. 
<br><br>
&nbsp;즉 토큰 방식으로 클라이언트를 인식한다는 것인데 이 방법의 가장 큰 장점은 서버에서 세션을 관리하지 않아 서버 자원을 아낄 수 있다는 것입니다. 또한 서버 간 유효성과 인증 검증 방식만 같다면 다중 서버를 운영하여도 문제가 없습니다. 이러한 점 때문에 이번 프로젝트에서 사용할려고 합니다. 개인적으로는 여러분들이 <b><a href="https://minwan1.github.io/2018/02/24/2018-02-24-OAuth/">Oauth</a></b>까지 무엇인지 알았으면 좋겠습니다. 대표적으로 JWT를 활용하는 보안 기술이고 주변에서 흔히 볼 수 있는 Google로 로그인, Naver로 로그인, Kakao로 로그인이 가능하게 한 기술이기 때문입니다.
<br><br>
<hr class="divider">
<h3>JWT의 활용</h3>
<br>
&nbsp;이제 스프링 시큐리티에 JWT를 활용한 방식을 설명할려고 합니다. 스프링 시큐리티에 대해 심화부분을 먼저 다루고 진행하고 싶지만 필자의 경험으로는 몸으로 부딪치고 나중에 머리로 이해하는 방법이 더 빠르다고 생각하기에 바로 연습부터 들어가겠습니다. 주로 참고한 사이트는 <b><a href="https://www.callicoder.com/series/spring-security-react/">여기</a></b>이며 코드를 설명하거나 소스를 다운받을 수 있는 링크가 있습니다. 우선 패키지 사용을 위해 의존성을 추가해야합니다.
<br><br>
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'

    // jwt
    compile 'io.jsonwebtoken:jjwt:0.9.0'
	compile 'com.fasterxml.jackson.datatype:jackson-datatype-jsr310'

	providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```
<br><br>
&nbsp;위 두 의존성은 JWT와 제이슨을 다루는데 도움을 주는 클래스를 제공해줍니다. 다음은 설정부분입니다.
<hr class="divider">
<h3>시큐리티 설정</h3>
<br>
<br><br>
```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true, jsr250Enabled = true, prePostEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private AccountService accountService;

    @Autowired
    private JwtAuthenticationEntryPoint unauthorizedHandler;

    @Bean
    public JwtAuthenticationFilter jwtAuthenticationFilter() {
        return new JwtAuthenticationFilter();
    }

    @Override
    public void configure(AuthenticationManagerBuilder authenticationManagerBuilder) throws Exception {
        authenticationManagerBuilder.userDetailsService(accountService) // 입력된 user에 대한 상세정보
                .passwordEncoder(passwordEncoder()); // password를 encorder하는 방식을 정함
    }

    @Bean(BeanIds.AUTHENTICATION_MANAGER)
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.cors().and().csrf().disable().exceptionHandling().authenticationEntryPoint(unauthorizedHandler).and()
                .authorizeRequests()
                .antMatchers("/", "/favicon.ico", "/**/*.png", "/**/*.gif", "/**/*.svg", "/**/*.jpg", "/**/*.html",
                        "/**/*.css", "/**/*.js")
                .permitAll().antMatchers("/api/auth/**", "/ws/**", "/api/mail/**").permitAll()
                .antMatchers("/api/account/checkEmailAvailability").permitAll()
                .antMatchers(HttpMethod.GET, "/api/polls/**", "/api/accounts/**").permitAll().anyRequest()
                .authenticated();
        // Add our custom JWT security filter
        http.addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class);

    }
}
```
<br><br>
&nbsp;이 클래스의 핵심을 잡자면 AuthenticationManager와 JwtAuthenticationFilter입니다.
<br><br>
![SpringSecutiryAuthenticationArchitecture](/files/security/SpringSecutiryAuthenticationArchitecture.png)
<br><br>
&nbsp;1번을 보면 클라이언트의 요청이 들어가고 있는데 AuthenticationFilter에서 이 요청의 형식이 적합한지 봅니다. 위에서 설정했다시피 "/api/auth/**"의 접근은 인증없이 허용한다던가하는 방식으로 필터에서 정할 수가 있습니다. 우리는 필터를 좀 더 추가하여 JWT에 대해 유효성을 검사하거나 발급하는 등에 사용하는 것입니다. 
AuthenticationManager란 인터페이스를 이용해 로그인 만든다고 생각해봅시다 AuthenticationProvider는 JWTTokenProvider가 되어 JWT를 발급해 줄 것이며 UserDetailService로 accountService를 등록할 것이고 그 서비스 안에서 UserDetails 인터페이스를 이용하여 인증으로 사용할 User 객체를 Account 객체로 사용할 것입니다. 
발급 받은 JWT은 AuthenticationFilter를 대체한 JwtAuthenticationFilter에서 유효성 검사를 받을 것이며, 검사를 끝맞치면 서버내에서 접근할 수 있는 것입니다.
<br><br>
&nbsp;이해가 어려운 사람들은 기존 세션-쿠키방식의 스프링 시큐리티방식을 JWT으로 전환하는데 아래의 작업이 필요하다는 걸 알면 될 것 같습니다. 
<hr class="divider">
<h3>시큐리티 필터</h3>
<br>
접근시 처음 거치는 것이 JwtAuthenticationFilter이니 바로 어떻게 구성되어 있는지 살펴보도록 하겠습니다.
<br><br>
```java
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Autowired
    private JwtTokenProvider tokenProvider;

    @Autowired
    private AccountService accountService;

    private static final Logger logger = LoggerFactory.getLogger(JwtAuthenticationFilter.class);

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        try {
            String jwt = getJwtFromRequest(request);

            if (StringUtils.hasText(jwt) && tokenProvider.validateToken(jwt)) {
                Long userId = tokenProvider.getUserIdFromJWT(jwt);

                UserDetails userDetails = accountService.loadUserById(userId);
                UsernamePasswordAuthenticationToken authentication = new UsernamePasswordAuthenticationToken(
                        userDetails, null, userDetails.getAuthorities());
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));

                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        } catch (Exception ex) {
            logger.error("Could not set user authentication in security context", ex);
        }
        filterChain.doFilter(request, response);
    }

    private String getJwtFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader("Authorization");
        if (StringUtils.hasText(bearerToken) && bearerToken.startsWith("Bearer ")) {
            return bearerToken.substring(7, bearerToken.length());
        }
        return null;
    }
}
```
<br><br>
&nbsp;여기서 요청의 헤더에서 JWT를 얻기 위해 "Bearer " 다음 문장을 찾는데 여기서 Bearer가 Bearer Token, 암호화하지 않은 전달 토큰이라는 의미입니다. 토큰 전달 방식 중 하나로 이해하면 됩니다. 그렇게 전달 받은 JWT를 이용해 DB내에 해당 유저가 있는지 비밀번호가 맞는지를 확인하고 UsernamePasswordAuthenticationToken 발급해 서버 내 SecurityContextHolder에서 보관하게 됩니다. 이제 유효성 검사와 토큰 발급 역할을 맏고 있는 JwtTokenProvider의 차례입니다.
<hr class="divider">
<h3>JWT 발급 클래스</h3>
<br>
<br><br>
```java
@Component
public class JwtTokenProvider {

    private static final Logger logger = LoggerFactory.getLogger(JwtTokenProvider.class);

    @Value("${app.jwtSecret}")
    private String jwtSecret; // 암호화 키

    @Value("${app.jwtExpirationInMs}")
    private int jwtExpirationInMs; // 만료일 상수

    public String generateToken(Authentication authentication) { //JWT 생성

        AccountPrincipal accountPrincipal = (AccountPrincipal) authentication.getPrincipal();

        Date now = new Date();
        Date expiryDate = new Date(now.getTime() + jwtExpirationInMs);
        return Jwts.builder()
                .setSubject(Long.toString(accountPrincipal.getId())) //PAYLOAD에 들어갈 sub
                .setIssuedAt(new Date()) // 생성일
                .setExpiration(expiryDate) // 만료일
                .signWith(SignatureAlgorithm.HS512, jwtSecret) // 암호화 방식
                .compact(); // 토큰 생성 메소드
    }

    public Long getUserIdFromJWT(String token) { // JWT로 부터 UserId 획득
        Claims claims = 
        Jwts
            .parser()
            .setSigningKey(jwtSecret)
            .parseClaimsJws(token)
            .getBody();

        return Long.parseLong(claims.getSubject());
    }

    public boolean validateToken(String authToken) { // JWT 유효성 검사
        try {
            Jwts.parser().setSigningKey(jwtSecret).parseClaimsJws(authToken);
            return true;
        } catch (SignatureException ex) {
            logger.error("Invalid JWT signature");
        } catch (MalformedJwtException ex) {
            logger.error("Invalid JWT token");
        } catch (ExpiredJwtException ex) {
            logger.error("Expired JWT token");
        } catch (UnsupportedJwtException ex) {
            logger.error("Unsupported JWT token");
        } catch (IllegalArgumentException ex) {
            logger.error("JWT claims string is empty.");
        }
        return false;
    }
}
```
<br><br>
&nbsp;구조가 간단하기에 대략적인 설명을 주석으로 달아놓았습니다. AccountPrincipal는 접근 주체(Principal)로 인증이 필요한 대상에 접근하는 유저로 UserDetail에서 좀 더 추가사항을 넣기 위해 만들어 사용하였습니다. JWT를 살펴보면 알다시피 암호화키와 해싱 알고리즘이 필요합니다. 일반적으로 ${app.jwtSecret}와 같이 프로퍼티 값을 application.yml파일에 선언하여 사용합니다. 
<br><br>
```yml
server:
    port: 8080

spring:
    h2:
        console:
            enabled: true
    jackson:
        time-zone: UTC
        serialization:
            WRITE_DATES_AS_TIMESTAMPS: false
        
logging:
    level:
        org:
            hibernate:
                SQL: debug
            springframework:
                security: debug
app:
    jwtSecret: JWTSuperSecretKey
    jwtExpirationInMs: 604800000

```
<br><br>
&nbsp;그 밖에 디버깅하기 편하도록 logging 프로퍼티와 jpa, security, json 프로퍼티를 선언하였습니다. 이제 로그인, 사인업 기능을 구현해 볼 차례입니다.
<hr class="divider">
<h3>로그인, 사인업 기능 구현</h3>
<br>
<br><br>
```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Autowired
    AuthenticationManager authenticationManager;

    @Autowired
    AccountRepository accountRepository;

    @Autowired
    RoleRepository roleRepository;

    @Autowired
    PasswordEncoder passwordEncoder;

    @Autowired
    JwtTokenProvider tokenProvider;

    @PostMapping("/login")
    public ResponseEntity<?> authenticateUser(@Valid @RequestBody SignRequest signRequest) {
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(signRequest.getEmail(), signRequest.getPassword()));

        SecurityContextHolder.getContext().setAuthentication(authentication);

        String jwt = tokenProvider.generateToken(authentication);
        return ResponseEntity.ok(new JwtAuthenticationResponse(jwt));
    }

    @PostMapping("/signup")
    public ResponseEntity<?> registerUser(@Valid @RequestBody SignRequest signRequest) {
        if (accountRepository.existsByEmail(signRequest.getEmail())) {
            return new ResponseEntity(new ApiResponse(false, "Username is already taken!"), HttpStatus.BAD_REQUEST);
        }
        // Creating user's account
        Account account = new Account(signRequest.getEmail(), signRequest.getPassword());

        account.setPassword(passwordEncoder.encode(account.getPassword()));

        Role userRole = roleRepository.findByName(RoleName.ROLE_USER)
                .orElseThrow(() -> new AppException("Account Role not set."));
        
        account.setRoles(Collections.singleton(userRole));

        Account result = accountRepository.save(account); 


        URI location = ServletUriComponentsBuilder.fromCurrentContextPath().path("/api/accounts/{email}")
                .buildAndExpand(result.getEmail()).toUri();
        return ResponseEntity.created(location).body(new ApiResponse(true, "Account registered successfully"));
    }
}
```
<br><br>
&nbsp;그 밖에 필요한 파일들을 링크에서 받아 완성한 후 실행해 보면 로그상에 어떤 필터가 적용되는지 확인 할 수 있습니다. 로그인을 위해 회원가입을 먼저 할려고 하지만 Role 생성에 문제가 있는지 Role를 정할 수 없다는 에러가 발생합니다. 데이터베이스를 확인할려고 H2 콘솔을 열어볼려하니 아래와 같은 오류가 생깁니다.
<br><br>
```
Refused to display 'http://localhost:8080/h2-console/query.jsp?jsessionid=b634f61cbb6732c889d818464177bbf8' in a frame because it set 'X-Frame-Options' to 'deny'.
```
<br><br>
&nbsp;스프링 시큐리티가 H2 콘솔에 접근하려하자 x-frame-options가 불가(deny)인걸 확인하고 화면표시를 하지 않기에 이러한 문제가 발생합니다. 이 문제를 해결하기 위해 다음 포스트에서 DB를 H2에서 MySQL로 전환하는 법에 대해 다루겠습니다. 
<div style="display:none;">
</div>