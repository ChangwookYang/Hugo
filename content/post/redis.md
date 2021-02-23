---
title: "Redis란?"
date: 2021-02-23T23:00:00+09:00
---

Redis는 REmote Dictionary Server의 약자로 "key-value" 기반 인메모리 비관계형 데이터베이스다.

모든 데이터를 메모리에 저장하고 조회하기떄문에 Read, Write 속도를 보장한다.

value에 다양한 자료구조를 지원하여 사용자 애플리케이션 개발 시에 활용도가 높다.

빠른 이유는 메모리 접근이 디스크 접근보다 빠르기 때문이다.



### Redis vs Memcached

Redis를 Memcached랑 많이 비교한다.

Memcached는 메모리 기반이라 처리속도가 빠르고 데이터에 만료 시간을 지정할 수 있고,

저장소 공간이 없으면 LRU에 의해 삭제되는 특징이 있어, 대형 포털에서

Static Page나 검색 결과 등 캐싱 용도로 많이 사용한다.

다만 프로세스가 죽거나 장비가 shutdown되면 저장된 데이터가 사라지기 때문에

중요 데이터를 저장하면 안된다.

Redis는 메모리 기반 빠른 처리 속도 등의 Memcached와 차이없는 처리속도를 보장함과 동시에

데이터를 디스크에도 저장해 영속화가 가능하여 데이터가 유실될 위험이 없다.

또한 문자열만 저장할 수 있는 Memcached와 달리 List, Set, Sorted Set, Hash 등을 지원한다.



### Redis Key

레디스 키는 문자열이기 때문에 'abc', jPEG 파일까지 모든 이진 시퀀스 키로 사용할 수 있다.

'user:1000'처럼 object-type:id의 형태를 권장



### Expire 기능

레디스는 in-memory DB인 만큼 메모리에 저장될 수있는 데이터는 한정적이다.

더 이상 메모리에 데이터를 저장할 수 없는 경우에는

레디스에서 가장 먼저 들어온 데이터를 삭제하거나, 가장 최근 사용되지 않은 데이터를 삭제하거나

혹은 데이터를 입력받지 못하게 해야한다.

가장 좋은 방법은 레디스가 선택하도록 맡기지 않고, 

데이터를 입력 할 때 이 데이터의 사용기한이 언제까지인지를 직접 설정해주는 것이다.

간단한 키에 대한 timeout을 설정하고 시간이 경과되면 키가 자동으로 삭제된다.

동일한 키가 다시 들어오면 이 timeout은 재설정된다.



### Spring에서 Redis 사용하기

Redis는 In-memory 기반의 NoSQL DBMS로서 Key-Value의 구조를 가지고 있다.

속도가 빠르고 사용이 간편하다.

캐싱, 세션관리, pub/sub 메시징 처리 등에 사용된다.

(pub/sub이란? https://sugerent.tistory.com/585)

  

Spring에서 Redis를 사용하기위한 라이브러리는 2가지가 있다.

* jedis
* lettuce

jedis는 thread-safe하지 않기 떄문에 jedis-pool을 사용해야한다.

그러나 비용이 증가하기 때문에 lettuce를 많이 사용한다.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```java
@Configuration
public class RedisConfiguration {
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        LettuceConnectionFactory lettuceConnectionFactory 
            = new LettuceConnectionFactory();
        return lettuceConnectionFactory;
    }
    
    @Bean
    public RedisTemplate<String, Object> redisTemplate(){
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new Jackson2Json )
        return redisTemplate;
    }
    
    @Bean
    MessageListener messageListenerAdapter() {
        return new MessageListenerAdapter(new RedisService());
    }
    
    @Bean
    RedisMessageListenerContainer redisContainer() {
        RedisMessageListenerContainer container = new RedisMessageListener();
    	container.setConnectionFactory(redisConnectionFactory());
        container.addMessageListener(messageListenerAdapter(), topic());
	    return container;
	}
    
    @Bean
    ChannelTopic topice() {
        return new ChannelTopic("Event");
    }
}
```

```java
@Service
public class RedisService implements MessageListener {
    public static List<String> messageList = new ArrayList<String>();
    
    @Override
    public void onMessage(Message message, byte[] pattern) {
        messageList.add(message.toString());
        sout("Message received : " + message.toString());
    }
}
```



* 아래 같이 설정하면 default로 localhost:6379로 연결

```properties
spring.redis.lettuce.pool.max-active=10
spring.redis.lettuce.pool.max-idle=10
spring.redis.lettuce.pool.min-idle=2
spring.redis.port=6379
spring.redis.host=127.0.0.1
```



### Redis 사용 예제





참고 : https://medium.com/garimoo/%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EB%A0%88%EB%94%94%EC%8A%A4-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-01-92aaa24ca8cc

 https://dydtjr1128.github.io/redis/2019/04/03/Redis.html