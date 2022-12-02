背景: 开发一个接口,有一个公积金授权编号,其规则是 YYYYMMddHHmmss+busiNo+八位随机数

问题: 如果在高并发情况下,八位随机数极其可能重复



目前解决方法:

使用redis生成递增id

```java
    public Integer generateUUID() {
        // redis key 设置为年月日时分(具体情况具体搞)
        RedisAtomicLong redisAtomicLong
                = new RedisAtomicLong(LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMddHHmm")),
                redisTemplate.getConnectionFactory());
        Long l = redisAtomicLong.incrementAndGet();
        redisAtomicLong.expire(70, TimeUnit.SECONDS);
        return l.intValue();
    }
```

