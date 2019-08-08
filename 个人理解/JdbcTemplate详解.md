# JDBCTemplate详解
---
先看下自带的的几个属性
```java
    //为True时忽略警告，否则为false时则抛出异常
	private boolean ignoreWarnings = true;
    //一次拉取数据的大小
	private int fetchSize = -1;
    //查询最大行数
	private int maxRows = -1;
    //查询超时时间
	private int queryTimeout = -1;

	private boolean skipResultsProcessing = false;
	
	private boolean skipUndeclaredResults = false;
	//返回结果集不区分大小写
	private boolean resultsMapCaseInsensitive = false;
```

* SingleColumnRowMapper
> 将结果转换为指定类型。将int.class这样子的类型转换为其封装类。
用这个转换数据时，结果集只能包含一个字段，都则抛出IncorrectResultSetColumnCountException，
默认使用JdbcUtils.getResultSetValue去处理返回的结果， 这里会根据期望类型进行转换，只转换基本类型及sql包的涉及到的类型和BigDecimal
当处理完毕之后校验当前返回结果类型是否和期望结果类型一致。不一致则进行数据转换
先看是否能转String，在看能否转Number，如果都不行则看是否注入conversionService，如果有则用conversionService进行转换。否则抛出异常


* ColumnMapRowMapper
> 将结果转换为一个LinkedCaseInsensitiveMap，这个Map注释上说是LinkedHashMap的变体，忽略的key的大小写。
实际上key全部转换为小写。


>方法太多就不一一的理了，但是这个的方法定义是有规律的。  
1. 返回单值的方法最后一定会在返回结果后调用DataAccessUtils.nullableSingleResult(results);
用来验证数据不能为空否则抛出EmptyResultDataAccessException
数据不能大于1条记录否则抛出IncorrectResultSizeDataAccessException
2. queryForObject()当指定期望类型时默认使用SingleColumnRowMapper来转换。否则自己指定RowMapper  
queryForList()当指定期望类型时默认使用SingleColumnRowMapper,不指定则使用ColumnMapRowMapper  
queryForMap()统一调用queryForObject(),指定使用ColumnMapRowMapper，因为使用的统一调用queryForObject所以返回的也是单值

```
public <T> T execute(StatementCallback<T> action);
```



```java
//查询返回一条记录，可以自定义一个RowMapper
queryForObject(String sql, RowMapper<T> rowMapper);
//查询返回一条记录，传入一个Class。用SingleColumnRowMapper封装
queryForObject(String sql, Class<T> requiredType)
/**
 上述方法 都调用了 public <T> List<T> query(String sql, RowMapper<T> rowMapper) 这个方法，
*/
public <T> T queryForObject(String sql, Object[] args, int[] argTypes, RowMapper<T> rowMapper)


/**
 
 */
```

```java
//requiredType用SingleColumnRowMapper封装
public <T> T queryForObject(String sql, Class<T> requiredType);
//默认使用ColumnMapRowMapper
public <T> List<T> queryForList(String sql, Class<T> elementType);

/**
 上述方法 都调用了 public <T> List<T> query(String sql, RowMapper<T> rowMapper) 这个方法，
*/
```