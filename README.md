# SHIRO-721 RememberMe Padding Oracle Vulnerability RCE

## 0x00 简介：

cookie的cookiememeMe已通过AES-128-CBC模式加密，这很容易受到填充oracle攻击的影响。

攻击者可以使用有效的RememberMe cookie作为Padding Oracle Attack 的前缀，然后制作精心制作的RememberMe来执行Java反序列化攻击，例如SHIRO-550。

## 0x01 重现此问题的步骤：

1. 登录网站，并从cookie中获取RememberMe。

2. 使用RememberMe cookie作为Padding Oracle Attack的前缀。

3. 加密ysoserial的序列化有效负载，以通过Padding Oracle Attack制作精心制作的RememberMe。

4. 请求带有新的RememberMe cookie的网站，以执行反序列化攻击。

5. 攻击者无需知道RememberMe加密的密码密钥。

## 0x02 影响版本

```
1.2.5, 
1.2.6, 
1.3.0, 
1.3.1, 
1.3.2, 
1.4.0-RC2, 
1.4.0, 
1.4.1
```
## rememberMeManager cipherKey
```
root@kali:/opt/tomcat/apache-tomcat-8.5.47/webapps/samples-web-1.5.0-SNAPSHOT# cat ./WEB-INF/shiro.ini |grep cipherKey

# We need to set the cipherKey, if you want the rememberMe cookie to work after restarting or on multiple nodes.

securityManager.rememberMeManager.cipherKey = kPH+bIxk5D2deZiIxcaaaA==

```
## default WEB-INF jar
```
./WEB-INF/lib/commons-beanutils-1.9.4.jar
./WEB-INF/lib/commons-codec-1.13.jar
./WEB-INF/lib/commons-collections-3.2.2.jar
./WEB-INF/lib/jcl-over-slf4j-1.7.26.jar
./WEB-INF/lib/log4j-1.2.17.jar
./WEB-INF/lib/shiro-cache-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-config-core-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-config-ogdl-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-core-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-crypto-cipher-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-crypto-core-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-crypto-hash-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-event-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-lang-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/shiro-web-1.5.0-SNAPSHOT.jar
./WEB-INF/lib/slf4j-api-1.7.26.jar
./WEB-INF/lib/slf4j-log4j12-1.7.26.jar
./WEB-INF/lib/taglibs-standard-impl-1.2.5.jar
./WEB-INF/lib/taglibs-standard-spec-1.2.5.jar
```

## 参考链接：

https://issues.apache.org/jira/browse/SHIRO-721

漏洞复现
https://www.anquanke.com/post/id/192819
