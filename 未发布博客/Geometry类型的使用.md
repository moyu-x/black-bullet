## 建表

MySQL 支持用于专门进行地理位置处理的函数，所以可以通过以下语句修改表结构，但是尽量应该设置为非空：

```
alter table location_table add geo geometry not null;
```

## 插入数据

### 对于用已有的数据生成

这个一遍是涉及到迁移已有的数据，这里只生成点的位置，日常常用的也是点的位置，数据表的 DDL 如下:

```
create table location(    id                       bigint auto_increment        primary key,    latitude                 double       null,    longitude                double       null,    geo                      geometry     null);
```

更新数据的语句如下:

```
update location lset l.geo = point(l.longitude, l.latitude)where 1 = 1;
```

### 插入新的数据

这个时候我没得从函数计算后保存成文本类型，具体如下：

```
insert into location values(geomfromtext('point(经度 维度)')
```

## 在 Java 中使用

### JPA 中使用

这里默认已经使用`Spring-Data-JPA`了，并且需要在配置中对中对`Hibernate`的方言进行声明，让其支持地理类型:

```
hibernate.dialect=org.hibernate.spatial.dialect.mysql.MySQL56SpatialDialect
```

### 依赖

```
<dependency>    <groupId>org.hibernate</groupId>    <artifactId>hibernate-spatial</artifactId>    <version>{hibernate-spatial.version}</version></dependency>
```

### Entity 的声明

```
@Entitypublic class LocationEntity {    @ID    @GeneratedValue    private Long id;    private Point geo;    // getter and setter}
```

### 保存

### Spring Data Jpa

在`Spring-data-Jpa`中，已经默认对 Point 进行了封装，可以直接使用

```
new Point(经度, 维度)
```

### Hibernate 自带

```
new WKTReader().read("POINT(" + 经度 + " " + 纬度 ")")
```

### 查询

以典型的判断一个点是不是在一个圆内：首先，画圆的方法：

```
public Geometry createCircle(double x, double y, double radius) {    GeometricShapeFactory shapeFactory = new GeometricShapeFactory();    shapeFactory.setNumPoints(32);    shapeFactory.setCentre(new Coordinate(x, y));    shapeFactory.setSize(radius * 2);    return shapeFactory.createCircle();}
```

然后查询语句，这个时候得使用`JPA`的`hsql`进行查询，以下是使用`Spring-data-JPA`的例子:

```
@Query("select s from location s where within(s.geo, :circle) = true")List<Location> queryExample(@Param("circle") Geometry circle)
```

### JOOQ 中的使用

声明其他和正式的一样，但是不同的的是这个时候已经不能使用 JOOQ 已经生成好的字段进行使用，这个时候得自己生成字段，使用`PlainSQL`生成模板，然后生成数据，工具类如下：

```
public static Condition stWithin(Field<?> left, Field<?> right) {    return DSL.condition("ST_WITHIN({0}, {1})", left, right);}
```

如果觉得这个太复杂，可以考虑实现`JOOQ Binding`这个就可以直接对数据库里的类型进行解析，可以从参考文档 5 中获取使用方法。

## 参考文档

1. [Introduction to Hibernate Spatial](https://www.baeldung.com/hibernate-spatial)
2. [https://www.jooq.org/doc/3.12/manual/sql-building/queryparts/custom-bindings/](https://www.jooq.org/doc/3.12/manual/sql-building/queryparts/custom-bindings/)
3. [https://stackoverflow.com/questions/38370831/how-to-select-points-within-polygon-in-postgis-using-jooq](https://stackoverflow.com/questions/38370831/how-to-select-points-within-polygon-in-postgis-using-jooq)
4. [https://zhuanlan.zhihu.com/p/62565501](https://zhuanlan.zhihu.com/p/62565501)
5. [https://github.com/jOOQ/jOOQ/issues/982](https://github.com/jOOQ/jOOQ/issues/982)