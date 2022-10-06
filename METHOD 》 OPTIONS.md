**METHOD**

```
OPTIONS 기능 막기
```

**web.xml 수정**

```
web.xml 파일 수정 : OPTIONS 기능 막기

<web-app>
    <security-constraint>
        <display-name>Forbidden</display-name>
        <web-resource-collection>
            <web-resource-name>Forbidden</web-resource-name>
            <url-pattern>/*</url-pattern>
               <http-method>OPTIONS</http-method>
        </web-resource-collection>
        <auth-constraint>
            <role-name></role-name>
        </auth-constraint>
    </security-constraint>
</web-app>
```

**확인**

```
* 적용 확인

> telnet 0 8080
OPTION / HTTP/1.0
```

**참조**

```
http://repository.egloos.com/v/5007347
```
