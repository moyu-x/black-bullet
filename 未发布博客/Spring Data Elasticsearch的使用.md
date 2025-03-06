## 版本对应关系

在使用前要注意Spring boot和Elasticsearch之间版本对应关系，免得再启动后有警告提示或者直接发生不能启动的情况

![[assets/4eb563c0859fc284e0d608cd62315a63_MD5.jpeg]]

## 使用前准备

如果是在本机进行开发和测试，只用使用docker启动一个单节点的ES服务就可以了，并且不需要进行任何的测试。

如果是在生产环境，还是使用Spring Cloud Config或者携程的Apollo其中之一进行配置的统一管理。

### Docker启动

```bash
docker run --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -d docker.elastic.co/elasticsearch/elasticsearch:7.9.2
```

### 基本配置

如果使用上述Docker方式启动用于开发应该是没有用户的，其他的基本配置如下：

```
spring.elasticsearch.rest.uris=https://search.example.com:9200
spring.elasticsearch.rest.read-timeout=10s
spring.elasticsearch.rest.username=user
spring.elasticsearch.rest.password=secret
```

## 使用

### 基本使用

对于基本的使用，于JPA类似，我们直接定义实体类，然后配置相应的`Repository`，在`Repository`中定义一些代理方法，最后就可以正常使用，下面是一个简单的代码示例：

```java
@Document(indexName = "user_index")
@Getter
@Setter
@ToString
@NoArgsConstructor
public class UserEsEntity{

    @Id
    @Nullable
    private String id;

    @Field(value = "last-name", type = FieldType.Keyword)
    private String lastName;

    @Field(type = FieldType.Keyword)
    private String type;
}

public interface UserEsRepository extends ElasticsearchRepository<UserEsEntity, String> {
}
```

### 复杂查询

对于增加和基本的查询，我们可以使用`Repository`就能完成，但是对于复杂的查询，在查看`ElasticsearchRepository`的相关类的注释后都发现这些代码已经被废弃，所以我们只能使用`ElasticsearchRestTemplate`来进行实现。

并且对于JPA代理类生成的如`List<UserEsEntity> findByLastNameContains(String text);`代码回会有以`0x`开头的乱码，所以不能正常的使用此方法进行查询，得使用`ElasticsearchRestTemplate`来进行查询。

所以得使用如下方式进行查询：

```java
@SpringBootTest
class WhitebirdElasticsearchDemoApplicationTests {

  @Autowired
	private ElasticsearchOperations elasticsearchOperations;
	@Autowired
	private ElasticsearchRestTemplate elasticsearchRestTemplate;  

  // 使用elasticsearchOperations进行查询
  @Test
	public void testElasticsearchOperations() {
		NativeSearchQuery searchQuery = new NativeSearchQueryBuilder()
				.withQuery(new MatchAllQueryBuilder())
				.build();
		SearchHits<UserEsEntity> searchHits = elasticsearchOperations.search(searchQuery, 
        UserEsEntity.class);
		List<SearchHit<UserEsEntity>> list = searchHits.getSearchHits();
		list.forEach(System.out::println);
	}

  @Test
	void testElasticsearchRestTemplate() {
		BoolQueryBuilder queryBuilder = QueryBuilders.boolQuery()
				.must(QueryBuilders.matchQuery("last-name", "测试"));
		NativeSearchQuery searchQuery = new NativeSearchQueryBuilder()
				.withQuery(queryBuilder)
				.build();
		SearchHits<UserEsEntity> search = elasticsearchRestTemplate.search(searchQuery, 
        UserEsEntity.class);
		search.forEach(System.out::println);
	}
}
```

### 含有聚合的查询

对于含有聚合的查询，还是继续使用原有的`ElasticsearchRestTemplate`来进行查询，但是再查询语句的查看方面会有一定的问题，查询语句的示例如下：

```java
  @Test
	void testAggregations() {

		BoolQueryBuilder queryBuilder = QueryBuilders.boolQuery();
		MatchQueryBuilder matchQueryBuilder = QueryBuilders.matchQuery("last-name", "2");
		RangeQueryBuilder ageRange = QueryBuilders.rangeQuery("age").from(1).to(20);

		queryBuilder.must(matchQueryBuilder).must(ageRange);

		NativeSearchQuery searchQuery = new NativeSearchQueryBuilder()
				.withQuery(queryBuilder)
				.withSearchType(SearchType.DEFAULT)
				.addAggregation(AggregationBuilders.terms("subjects").field("user-type"))
				.build();

		SearchHits<UserEsEntity> search = elasticsearchRestTemplate.search(searchQuery, 
        UserEsEntity.class, IndexCoordinates.of("user_index"));

		search.forEach(System.out::println);

	}
```

### 查询语句查看

对于普通不使用聚合查询的方式，可以直接从NativeSearchQuery中获取查询语句并打印的方式来进行查询

```java
System.out.println(searchQuery.getQuery().toString());
```

对于使用了聚合查询的文章，需要使用如下的方式查看整个查询日志：

```yaml
logging:
  level:
    org.elasticsearch.client: trace
    org.apache.http: trace
```

## 参考文章

1. [Introduction to Spring Data Elasticsearch](https://www.baeldung.com/spring-data-elasticsearch-tutorial)
2. [Elasticsearch Queries with Spring Data](https://www.baeldung.com/spring-data-elasticsearch-queries)