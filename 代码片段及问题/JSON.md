## FastJson转带泛型Map

```java
JSONObject.parseObject(rs, new TypeReference<HashMap<String, Long>>() {});
```