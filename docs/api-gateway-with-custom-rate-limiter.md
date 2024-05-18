# API Gateway with Custom Rate Limiter

> **[ 요구 사항 ] **
> 1. 기본적인 라우팅 기능
> 2. 라우터마다 처리율 제한을 다르게 설정할 수 있어야 함.


## Spring Cloud Gateway
1. Netty 기반으로 request 를 처리.
   - SPRING MVC 는 1 thread / 1 request
   - Netty 는 1 thread / N request
2. gateway 동작을 yaml 로 쉽게 조절할 수 있음.
3. 기본적으로 `RedisRateLimiter` 라는 객체를 제공한다.


## RedisRateLimiter
1. redis + lua script + 토큰 버킷 알고리즘을 사용함.
2. 각 알고리즘마다의 CustomRateLimiter 객체를 만들어둔다.   
`RedisRateLimiter` 객체와 `Rate` 객체를 커스텀하여 구현할 수 있다.


---
참고자료
- https://saramin.github.io/2022-01-20-spring-cloud-gateway-api-gateway/
- https://medium.com/@animeshchaturvedi007/custom-rate-limiting-with-sliding-window-using-spring-cache-and-spring-boot-91231a9c95a9