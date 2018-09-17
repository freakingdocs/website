---
title: "Form Auth and Https"
date: 2018-09-17T13:45:49-07:00
anchor: "form-auth-and-https"
weight: 
---

By default, you probably want HTTPS and some sort of simple form authentication on Cloud Foundry to get you going.

We use the `cloud` profile which you get for free when pushing to CF. 

The `{noop}` modifier in the password uses a deprecated [NoOpPasswordEncoder](https://docs.spring.io/spring-security/site/docs/4.2.7.RELEASE/apidocs/org/springframework/security/crypto/password/NoOpPasswordEncoder.html)

```java
@Profile("cloud")
@EnableWebSecurity
public class SecurityCloudConfiguration extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http
        .requiresChannel().anyRequest().requiresSecure()
        .and()
        .authorizeRequests()
        .antMatchers("/api/**").hasRole("USER")
        .and()
        .formLogin();
  }

  @Autowired
  public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
    auth
        .inMemoryAuthentication()
        .withUser("user").password("{noop}cool-password!").roles("USER");
  }
}
```
