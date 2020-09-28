---
title: Jwt权限控制api 访问
description: 正文被隐藏
date: 2020-09-26 21:24:35
tags:  jwt令牌
categories: jwt
---
给一个api 上添加 `@ScopeLevel`   来确定这个api 的等级

```java
   @GetMapping("/name/{name}")
    @ScopeLevel
    public Banner getByName(@PathVariable @NotBlank String name){
        Banner banner = bannerService.getByName(name);
        if (banner==null){
            throw new NotFoundException(30005);
        }
        return banner;
    }
```



`@ScopeLevel` 注解:

```java
@Target({ElementType.TYPE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface ScopeLevel {
    int value() default 4;
}

```



之所以可以完成是因为:

1. 通过`TokenController` 获取token

```java
@RequestMapping(value = "/v1/token")
@RestController
public class TokenController {

    @Autowired
    private WxAuthenticationService wxAuthenticationService;

    //获取token
    @PostMapping("")
    public Map<String,String> getToken(@RequestBody @Validated TokenGetDTO userData){

        Map<String,String> map = new HashMap<>();
        String token = null;

        switch (userData.getType()){
            case USER_WX:
                token = wxAuthenticationService.code2Session(userData.getAccount());
                break;
            case USER_Email:break;
            default: throw new NotFoundException(10003);
        }

        map.put("token",token);
        return map;
    }
    
    // 验证token
    @PostMapping("/verify")
    public Map<String, Boolean> verify(@RequestBody TokenDTO token) {
        Map<String, Boolean> map = new HashMap<>();
        Boolean valid = JwtToken.verifyToken(token.getToken());
        map.put("is_valid", valid);
        return map;
    }


}

```



`TokenGetDTO`

```java
@Getter
@Setter
public class TokenGetDTO {

    @NotBlank(message = "account 不允许为空")
    private String account;
    @TokenPassword(message="{token.password}")
    private String password;
    private LoginType type;
}
```

`LoginType`

```java
public enum LoginType {
    USER_WX(0,"微信登录"),
    USER_Email(1,"邮箱登录");

    private Integer value;
    LoginType(Integer value,String description){
        this.value = value;
    }
    public void test(){}
}
```

`WxAuthenticationService`

```java
@Service
public class WxAuthenticationService {

    @Autowired
    private ObjectMapper mapper;  //用于反序列化 .readValue()方法

    @Autowired
    private UserRepository userRepository;


    //用appid  secret  code 码 换取session
    @Value("${wx.code2session}") 
    //https://api.weixin.qq.com/sns/jscode2session?appid={0}&secret={1}&js_code={2}&grant_type=authorization_code
    private String code2SessionUrl;
    @Value("${wx.appid}")
    private String appid;
    @Value("${wx.appsecret}")
    private String appsecret;

    public String code2Session(String code) {

		//messageFormat.format 模板填充{0}{1}{2},组装成一个可以使用的url.
        String url = MessageFormat.format(this.code2SessionUrl, this.appid, this.appsecret, code);
        //RestTemplate 用来发送http 请求
        RestTemplate rest = new RestTemplate();
        //发送请求 
        String sessionText = rest.getForObject(url, String.class);
        Map<String, Object> session = new HashMap<>();
        try {
            //将String 序列化到session 中 
            //访问微信服务器正确将得到
            //{"appid":"xxx","session":"xxx"}
            session = mapper.readValue(sessionText, Map.class);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return this.registerUser(session);
    }

    //发放令牌
    private String registerUser(Map<String, Object> session) {
        String openid = (String) session.get("openid");
        if (openid == null) {
            //如果没获取到openid,抛出获取用户openid失败异常
            throw new ParameterException(20004);
        }
        //通过openid 获得这个user
        Optional<User> userOptional = this.userRepository.findByOpenid(openid);
        if (userOptional.isPresent()) {
			// 入如果user不为空,获取id 颁发令牌
            return JwtToken.makeToken(userOptional.get().getId());
        }
        //user 为空 先注册到user 表
        User user = User.builder().openid(openid)
                .build();
        userRepository.save(user);
        // 再次重新获得该用户id ,颁发令牌
        Long uid = user.getId();
        return JwtToken.makeToken(uid);
    }
}
```



`UserRepository`

```java
public interface UserRepository extends JpaRepository<User, Long> {
    User findByEmail(String email);
    Optional<User> findByOpenid(String openid);
    User findFirstById(Long id);
    User findByUnifyUid(Long uuid);
}

```

`jwtToken`    令牌生成具体方法//    默认可访问的等级是8   8以下的都可以被访问

```java
@Component
public class JwtToken {

    private static String jwtKey;
    private static Integer expiredTimeIn;
    private static Integer defaultScope = 8;

    //密钥 :  yml 配置的
    //security:
    //	jwt-key: sssssssfan
    //	token-expired-in: 86400
    @Value("${security.jwt-key}")
    public void setJwtKey(String jwtKey) {
        JwtToken.jwtKey = jwtKey;
    }

    @Value("${security.token-expired-in}")
    public void setExpiredTimeIn(Integer expiredTimeIn) {
        JwtToken.expiredTimeIn = expiredTimeIn;
    }

    public static Optional<Map<String, Claim>> getClaims(String token) {
        DecodedJWT decodedJWT;
        Algorithm algorithm = Algorithm.HMAC256(JwtToken.jwtKey);
        JWTVerifier jwtVerifier = JWT.require(algorithm).build();
        try {
            decodedJWT = jwtVerifier.verify(token);
        } catch (JWTVerificationException e) {
            return Optional.empty();
        }
        return Optional.of(decodedJWT.getClaims());
    }

    public static Boolean verifyToken(String token) {
        try {
            Algorithm algorithm = Algorithm.HMAC256(JwtToken.jwtKey);
            JWTVerifier verifier = JWT.require(algorithm).build();
            verifier.verify(token);
        } catch (JWTVerificationException e) {
            return false;
        }
        return true;
    }


    public static String makeToken(Long uid, Integer scope) {
        return JwtToken.getToken(uid, scope);
    }

    public static String makeToken(Long uid) {
        return JwtToken.getToken(uid, JwtToken.defaultScope);
    }
	// 颁发jwt 令牌
    private static String getToken(Long uid, Integer scope) {
        //使用HMAC256 算法 对key 加密
        Algorithm algorithm = Algorithm.HMAC256(JwtToken.jwtKey);
        Map<String, Date> map = JwtToken.calculateExpiredIssues();
		
        // 颁发jwt
        return JWT.create()
                .withClaim("uid", uid)
                .withClaim("scope", scope)
                .withExpiresAt(map.get("expiredTime"))
                .withIssuedAt(map.get("now"))
                .sign(algorithm);
    }

    private static Map<String, Date> calculateExpiredIssues() {
        Map<String, Date> map = new HashMap<>();
        Calendar calendar = Calendar.getInstance();
        Date now = calendar.getTime(); //当前时间
        calendar.add(Calendar.SECOND, JwtToken.expiredTimeIn); // 过期时间
        map.put("now", now);
        map.put("expiredTime", calendar.getTime());
        return map;
    }
}

```

用拦截器获取所有的http 访问:

`PermissionInterceptor`

```java
public class PermissionInterceptor extends HandlerInterceptorAdapter {
//或者     implement  HandlerInterceptor  都可以
    public PermissionInterceptor(){

    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
		// 获取注解 scopelevel的值
        Optional<ScopeLevel> scopeLevel = this.getScopeLevel(handler);
        // 不存在 任何人可访问
        if (!scopeLevel.isPresent()){
            return true;
        }
        // 获取 Header Authorization 的值,也就是 token值
        String bearerToken = request.getHeader("Authorization");
        //空值 报不合法异常
        if (StringUtils.isEmpty(bearerToken)){
            throw new UnAuthenticatedException(10004);
        }
        //非jwt Bearer 开头的令牌 不合法异常
        if (!bearerToken.startsWith("Bearer")){
            throw new UnAuthenticatedException(10004);
        }
        // 防止非法输入   输入中带空格
        String tokens[] = bearerToken.split(" ");
        if (!(tokens.length ==2)){
            throw new UnAuthenticatedException(10004);
        }
        //提取token  tokens[0] 值是Bearer
        String token = tokens[1];
        //验证token
        //optionalMap :: optional[{uid=com.auth0.jwt.impl.JsonNodeClaim@7211e007, exp=com.auth0.jwt.impl.JsonNodeClaim@ea3d01c, iat=com.auth0.jwt.impl.JsonNodeClaim@25cfbd5, scope=com.auth0.jwt.impl.JsonNodeClaim@4e547dbe}]
        Optional<Map<String, Claim>> optionalMap = JwtToken.getClaims(token);
        //将optionalMap 的值放入 map
        Map<String, Claim> map = optionalMap.orElseThrow(() -> new UnAuthenticatedException(10004));
        boolean valid = hasPermission(scopeLevel.get(), map);
        return valid;
    }
    
    /**
     * 获取ScopeLevel 的值
     * @param handler
     * @return
     */
    private Optional<ScopeLevel> getScopeLevel(Object handler){
        if (handler instanceof HandlerMethod) {
            HandlerMethod handlerMethod = (HandlerMethod) handler;
            ScopeLevel scopeLevel = handlerMethod.getMethod().getAnnotation(ScopeLevel.class);
            if (scopeLevel == null){
                return Optional.empty();
            }
            return  Optional.of(scopeLevel);
        }
        return Optional.empty();
    }
    
    
    //判断 scopeLevel 和 defaultscope (8) 的大小. 超过8 不能访问
    // 当给一个类加上@ScopeLevel() 默认4 
    // 可以做到 没有登录访问不了api
    private boolean hasPermission(ScopeLevel scopeLevel, Map<String, Claim> map) {
        Integer level = scopeLevel.value();
        Integer scope = map.get("scope").asInt();
        if (level > scope) {
            throw new ForbiddenException(10005);
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {



        super.postHandle(request, response, handler, modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {


        super.afterCompletion(request, response, handler, ex);
    }

    
}
```

将拦截器加入到(注册到)系统,也就是成功拦截一切的http 访问

`PermissionInterceptor`

```java
@Component
public class InterceptorConfiguration implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new PermissionInterceptor());
    }
}

```

总结: 

1. 用户登录 颁发jwt 令牌 ,该令牌中 放入了uid 和 scope 值     ``JwtToken.makeToken()`
2. 用拦截器获取任何http访问 ``PermissionInterceptor`  `PermissionInterceptor`
3. 拦截器中获取自定义注解@ScopeLevel 的值  `PermissionInterceptor`中的`getScopeLevel() `方法
   1. 为空值,所有权限可访问
4. 在Header的 `Authorization`  获取token   `PermissionInterceptor`的 ` preHandle  .     request.getHeader("Authorization")    `
5. 获取token 的   scope   `JwtToken.getClaims(token).get("scope")`
6. 比较  level 和 scope 值,判断是否可以访问 `PermissionInterceptor`.`hasPermission()`

 