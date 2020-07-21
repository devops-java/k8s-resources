# Replication Controller / Replica Set / Daemon Set

### Liveness

```
kubectl apply -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-rc/pod-liveness.json
$ kubectl get pods
NAME              READY   STATUS    RESTARTS   AGE
spring-boot-web   1/1     Running   2          3m54s

kubectl describe po spring-boot-web

Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  4m53s                  default-scheduler  Successfully assigned default/spring-boot-web to minikube
  Normal   Pulling    4m51s                  kubelet, minikube  Pulling image "rkp201/k8s-spring-app:1.5"
  Normal   Pulled     4m30s                  kubelet, minikube  Successfully pulled image "rkp201/k8s-spring-app:1.5"
  Warning  Unhealthy  2m51s (x2 over 4m21s)  kubelet, minikube  Liveness probe failed: Get http://172.18.0.4:8080/health: dial tcp 172.18.0.4:8080: connect: connection refused
  Normal   Created    90s (x3 over 4m29s)    kubelet, minikube  Created container spring-boot-web
  Normal   Started    90s (x3 over 4m29s)    kubelet, minikube  Started container spring-boot-web
  Warning  Unhealthy  11s (x9 over 3m20s)    kubelet, minikube  Liveness probe failed: HTTP probe failed with statuscode: 500
  Normal   Killing    11s (x3 over 3m)       kubelet, minikube  Container spring-boot-web failed liveness probe, will be restarted
  Normal   Pulled     10s (x3 over 2m59s)    kubelet, minikube  Container image "rkp201/k8s-spring-app:1.5" already present on machine
  
events showing pod is getting restarted due to unhealthy.  


When pod is reatrted we can see the previous failuer logs.

$ kubectl logs spring-boot-web --previous

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/ :: Spring Boot ::        (v2.3.1.RELEASE)
2020-07-21 08:02:27.971  INFO 1 --- [           main] c.e.SpringDockerImageAppApplication      : Starting SpringDockerImageAppApplication v0.0.1-SNAPSHOT on spring-boot-web with PID 1 (/app.jar started by root in /)
2020-07-21 08:02:27.976  INFO 1 --- [           main] c.e.SpringDockerImageAppApplication      : Noactive profile set, falling back to default profiles: default
2020-07-21 08:02:29.621  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2020-07-21 08:02:29.640  INFO 1 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2020-07-21 08:02:29.641  INFO 1 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.36]
2020-07-21 08:02:29.769  INFO 1 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2020-07-21 08:02:29.769  INFO 1 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1727 ms
2020-07-21 08:02:30.313  INFO 1 --- [           main] o.s.s.concurrent.ThreadPoolTaskExecutor  : Initializing ExecutorService 'applicationTaskExecutor'
2020-07-21 08:02:30.560  INFO 1 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2020-07-21 08:02:30.571  INFO 1 --- [           main] c.e.SpringDockerImageAppApplication      : Started SpringDockerImageAppApplication in 3.306 seconds (JVM running for 4.036)
2020-07-21 08:02:36.030  INFO 1 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2020-07-21 08:02:36.031  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2020-07-21 08:02:36.038  INFO 1 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 7 ms
2020-07-21 08:03:25.948 ERROR 1 --- [nio-8080-exec-6] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is java.lang.RuntimeException: error in health check] with root cause

java.lang.RuntimeException: error in health check
        at com.example.controller.TestController.getErrorCount(TestController.java:36) ~[classes!/:0.0.1-SNAPSHOT]
        at com.example.controller.TestController.health(TestController.java:17) ~[classes!/:0.0.1-SNAPSHOT]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_212]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_212]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_212]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_212]
        at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:105) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:879) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:793) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:634) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) ~[tomcat-embed-websocket-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:139) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.RemoteIpValve.invoke(RemoteIpValve.java:747) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:373) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:868) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1590) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [na:1.8.0_212]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [na:1.8.0_212]
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.lang.Thread.run(Thread.java:748) [na:1.8.0_212]

2020-07-21 08:03:35.943 ERROR 1 --- [nio-8080-exec-7] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is java.lang.RuntimeException: error in health check] with root cause

java.lang.RuntimeException: error in health check
        at com.example.controller.TestController.getErrorCount(TestController.java:36) ~[classes!/:0.0.1-SNAPSHOT]
        at com.example.controller.TestController.health(TestController.java:17) ~[classes!/:0.0.1-SNAPSHOT]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_212]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_212]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_212]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_212]
        at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:105) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:879) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:793) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:634) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) ~[tomcat-embed-websocket-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:139) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.RemoteIpValve.invoke(RemoteIpValve.java:747) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:373) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:868) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1590) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [na:1.8.0_212]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [na:1.8.0_212]
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.lang.Thread.run(Thread.java:748) [na:1.8.0_212]

2020-07-21 08:03:45.933 ERROR 1 --- [nio-8080-exec-8] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is java.lang.RuntimeException: error in health check] with root cause

java.lang.RuntimeException: error in health check
        at com.example.controller.TestController.getErrorCount(TestController.java:36) ~[classes!/:0.0.1-SNAPSHOT]
        at com.example.controller.TestController.health(TestController.java:17) ~[classes!/:0.0.1-SNAPSHOT]
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:1.8.0_212]
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[na:1.8.0_212]
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:1.8.0_212]
        at java.lang.reflect.Method.invoke(Method.java:498) ~[na:1.8.0_212]
        at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:105) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:879) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:793) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:898) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:634) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883) ~[spring-webmvc-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at javax.servlet.http.HttpServlet.service(HttpServlet.java:741) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53) ~[tomcat-embed-websocket-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119) ~[spring-web-5.2.7.RELEASE.jar!/:5.2.7.RELEASE]
        at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202) ~[tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:541) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:139) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.valves.RemoteIpValve.invoke(RemoteIpValve.java:747) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:373) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:65) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:868) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1590) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) [na:1.8.0_212]
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) [na:1.8.0_212]
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) [tomcat-embed-core-9.0.36.jar!/:9.0.36]
        at java.lang.Thread.run(Thread.java:748) [na:1.8.0_212]

2020-07-21 08:03:45.991  INFO 1 --- [extShutdownHook] o.s.s.concurrent.ThreadPoolTaskExecutor  : Shutting down ExecutorService 'applicationTaskExecutor'

With InitialDelaySeconds:

kubectl apply -f https://raw.githubusercontent.com/devops-java/k8s-resources/master/pod-rc/pod-liveness-v1.json
```
