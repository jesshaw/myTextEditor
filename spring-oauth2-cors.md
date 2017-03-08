# spring oauth2 cors
## 最近被跨域问题困扰数天，经过多方查找和学习，如下方案可以彻底解决跨域问题
1. 增加支持跨域的过滤器CORSFilter
2. 确保继承自AuthorizationServerConfigurerAdapter的实现类覆写configure(AuthorizationServerSecurityConfigurer oauthServer)方法是增加跨域过滤器实例
3. 确保继承自WebSecurityConfigurerAdapter的实现类覆写configure(HttpSecurity http)时增加antMatchers(HttpMethod.OPTIONS, "/oauth/token").permitAll()。
4. 对于ajax的终端因会事先发OPTIONS请求，因此在跨域的过滤器CORSFilter，需针对OPTIONS做特殊处理，须让其正确返回

## 如下为可能涉及到的相关知识点及示例

[参考1：实现跨域的几种方式](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/cors.html)

[参考2：针对oath2需做的特殊处理](http://stackoverflow.com/questions/25136532/allow-options-http-method-for-oauth-token-request)

[参考3：oauth2示例](http://websystique.com/spring-security/secure-spring-rest-api-using-oauth2/)

[参考4：实现跨域的别一种方式](http://stackoverflow.com/questions/30632200/standalone-spring-oauth2-jwt-authorization-server-cors/30638914#30638914)