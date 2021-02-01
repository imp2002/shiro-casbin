# jcasbin-shiro-plugin
Apache Shiro's RBAC &amp; ABAC Authorization Plug-in based on jCasbin

## How it works?

**1.Add configuration**

You need to specify the path of the model file in the configuration file.

```yaml
shiro-jcasbin:
  // madel path
  modelPath: src/test/resources/model/rbac_model.conf
````

Of course, you also need to configure the data source information in the spring configuration file. For example:

```yaml
shiro-jcasbin:
  // madel path
  modelPath: src/test/resources/model/rbac_model.conf

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/casbin?serverTimezone=GMT%2B8
    username: casbin_test
    password: TEST_casbin
```

**2.Enable annotation interception**

You need to enable annotation interception in Shiro's configuration file.As for annotation startup classes, please use the ShiroAdvisor, like this:

```java
@Configuration
public class ShiroConfig {
    // Other configs is omitted.
    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor advisor = new ShiroAdvisor();
        advisor.setSecurityManager(securityManager);
        return advisor;
    }
}
```

**3.Use the EnforcerAuth annotation.**

Now you can use the EnforcerAuth annotation to the controller method, like this:
```java
@RestController
public class EnforcerAuthController {
    // Other methods is omitted.
    @EnforcerAuth
    @GetMapping("/data")
    public String function1() {
        return "success";
    }
}
```

It will verify whether the current login user has the requested address permission. If the current login user has the permission, this controller method will work properly. If not, it will throw the exception which can prompt you that the current login user does not have the permission.If no user is currently logged in, it will throw the exception which can prompt you that you should login in.
