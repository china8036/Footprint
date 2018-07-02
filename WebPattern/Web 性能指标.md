- [Web 性能指标](#web-性能指标)
	- [1. Refer Links](#1-refer-links)

# Web 性能指标

- PV (Page View)

- QPS (Query Per Second) / QPM (Query Per Minute): 应用服务器每秒 / 分钟能处理的查询次数。

	> QPS = 总请求数 / 请求时间

- 峰值 QPS: 每天 80% 的访问集中在 20% 的时间里，这 20% 时间叫做峰值时间，这段时间内的 QPS 称为峰值 QPS。

	> 峰值 QPS = ( 总 PV 数 * 80% ) / ( 每天秒数 * 20% )

	问：每天 300w PV 的在单台机器上，这台机器需要多少 QPS？如果一台机器的 QPS 是 58，需要几台机器来支持？
	
	答：
	- ( 3000000 * 0.8 ) / (86400 * 0.2 ) = 139 (QPS)
	- 139 / 58 = 3 台

- RPS (Requests Per Second)

- TPS (Transactions Per Second): 每秒传输的事物处理个数，即服务器每秒处理的事务数。TPS 包括一条消息入和一条消息出，加上一次用户数据库访问。

## 1. Refer Links

[关于服务性能优化思考](https://blog.csdn.net/Weiguang_123/article/details/60633276)

[PV、TPS、QPS 是怎么计算出来的？](https://www.zhihu.com/question/21556347)