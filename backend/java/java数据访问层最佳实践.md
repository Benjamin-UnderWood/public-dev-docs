# java 数据访问层最佳实践

## JPA 和 JdbcTemplate

- 对数据的增，删，改主要使用 JPA（底层基于 hibernate 的实现)，简单的查询也使用 JPA;

  - 优点：简单，可读性好

- 如果遇到一定需要手写 update 语句的场景，使用@Query（value = "update ...", nativeQuery = true)，再配上@Modifying 和@Transactional

  - 优点：执行修改语句更灵活，想更新什么字段就更新什么字段

- 对于复杂的查询，比如条件比较复杂，或者用嵌套的子查询，分页等，使用 JdbcTemplate；

  - 优点：使用原生 sql 语句，更灵活

## 一些推荐的代码写法

- 在@Query 中返回 bool 值

  使用 case when 语法:

```java
  /**
   * 根据航司代码查看是否存在（排除指定的id)
   */
  @Query("select case when count(id) > 0 then true else false end from CommonAirline  where code = :code and id <> :exceptId")
  boolean existsByCodeAndExceptId(@Param("code") String code,
                                  @Param("exceptId") Long exceptId);
```
