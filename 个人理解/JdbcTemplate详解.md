# JDBCTemplate详解
---
先看下自带的的几个属性
```java
    //为True时忽略警告，否则为false时则抛出异常
	private boolean ignoreWarnings = true;
    //FetchSize最主要是为了减少网络交互次数设计的。 访问 ResultSet时，
	//如果它每次只从 服务器上读取一行数据，则会产生大量的开销。
	//setFetchSize的意思是当调用 rs.next时，ResultSet 会一次性从服务器上取得多少行数据回来，
	//这样在下次 rs.next时， 它可以直接从内存中获取数 据而不需要网络交互， 提高了效率。 
	//这个设置可能会被某些 JDBC 驱动忽略，而且设置过大 也会造成内存的上升 。
	private int fetchSize = -1;
    //MaxRows 将此 Statement对象生成的所有 ResultSet对象可以包含的最大行数限制设置为给定数。
	private int maxRows = -1;
    //查询超时时间
	private int queryTimeout = -1;

	private boolean skipResultsProcessing = false;
	
	private boolean skipUndeclaredResults = false;
	//返回结果集不区分大小写
	private boolean resultsMapCaseInsensitive = false;
```
---
## 回调接口
* StatementCallback
> 通过回调获取JdbcTemplate提供的Statement，用户可以在该Statement执行任何数量的操作

* PreparedStatementCallback
> 通过回调获取JdbcTemplate提供的PreparedStatement，用户可以在该PreparedStatement执行任何数量的操作,一般会配合PreparedStatementSetter使用。

* CallableStatementCallback
> 通过回调获取JdbcTemplate提供的CallableStatement，用户可以在该CallableStatement执行任何数量的操作

* ConnectionCallback
> 很少使用。看代码中的例子好像是对Connection又做了一下处理。
---
## Statement创建相关
* PreparedStatementCreator
> 用于创建PreparedStatement

* CallableStatementCreator
> 用于创建CallableStatement

**普通的Statement直接通过stmt = con.createStatement();创建**   

---
## 参数赋值及批处理相关  
* PreparedStatementSetter
> 相应的预编译语句相应参数赋值。

* ArgumentPreparedStatementSetter
> 只给定参数即可  

* ArgumentTypePreparedStatementSetter
> 给定参数与参数类型

* BatchPreparedStatementSetter
> 用于批处理，需要指定批处理大小

---
## 结果集转换
* ResultSetExtractor
> 用于结果集数据提取，用户需实现方法extractData(ResultSet rs)来处理结果集

* *  SqlRowSetResultSetExtractor 
	> 标准的CachedRowSet 不经常使用不做研究，相关博客->[CachedRowSet使用](https://www.cnblogs.com/wxgblogs/p/6203338.html?utm_source=itdadao&utm_medium=referral)

	* RowMapperResultSetExtractor
	> Adapter implementation 将转换方法委托给RowMapper

	* RowCallbackHandlerResultSetExtractor
	> Adapter implementation 将转换方法委托给RowCallbackHandler

* RowCallbackHandler
> 用于处理ResultSet的每一行结果，用户需实现方法processRow(ResultSet rs)来完成处理，在该回调方法中无需执行rs.next()，该操作由JdbcTemplate来执行，用户只需按行获取数据然后处理即可

* * RowCountCallbackHandler
	> 看JavaDoc感觉像是用来方便拿取RowCount。

* RowMapper
> 用于将结果集每行数据转换为需要的类型，用户需实现方法mapRow(ResultSet rs, int rowNum)来完成将每行数据转换为相应的类型。  

* * SingleColumnRowMapper
	> 将结果转换为指定类型。将int.class这样子的类型转换为其封装类。
	用这个转换数据时，结果集只能包含一个字段，都则抛出IncorrectResultSetColumnCountException，
	默认使用JdbcUtils.getResultSetValue去处理返回的结果， 这里会根据期望类型进行转换，只转换基本类型及sql包的涉及到的类型和BigDecimal
	当处理完毕之后校验当前返回结果类型是否和期望结果类型一致。不一致则进行数据转换
	先看是否能转String，在看能否转Number，如果都不行则看是否注入conversionService，如果有则用conversionService进行转换。否则抛出异常  

	* ColumnMapRowMapper
	> 将结果转换为一个LinkedCaseInsensitiveMap，这个Map注释上说是LinkedHashMap的变体，忽略的key的大小写。
	实际上key全部转换为小写。

---
>方法太多就不一一的理了，但是这个的方法定义是有规律的。  
1. 返回单值的方法最后一定会在返回结果后调用DataAccessUtils.nullableSingleResult(results);
用来验证数据不能为空否则抛出EmptyResultDataAccessException
数据不能大于1条记录否则抛出IncorrectResultSizeDataAccessException
2. queryForObject()当指定期望类型时默认使用SingleColumnRowMapper来转换。否则自己指定RowMapper  
queryForList()当指定期望类型时默认使用SingleColumnRowMapper,不指定则使用ColumnMapRowMapper  
queryForMap()统一调用queryForObject(),指定使用ColumnMapRowMapper，因为使用的统一调用queryForObject所以返回的也是单值   
3. 无需预编译的SQL，直接是SQL+RowMapper,或者期望类型，需要预编译的SQL则需要再加参数和参数类型