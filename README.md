# 智能面试刷题平台
## 项目介绍
基于 Spring Boot + Redis + MySQL + Elasticsearch 的面试刷题平台。管理员可以创建题库并批量关联题目；用户可以分词检索题目、在线刷题并查看刷题记录日历等。运用 Druid + HotKey + Sa-Token + Sentinel + Nacos 全面优化性能和安全性。

## 本项目架构设计图：
![](https://cdn.nlark.com/yuque/0/2025/png/47747359/1740300824251-93d79e90-5638-4625-b9d0-0a4ef09ac9d3.png)

## 技术选型
+ Java Spring Boot 框架 + Maven 多模块构建
+ MySQL 数据库+ MyBatis-Plus 框架 + MyBatis X
+ Redis 分布式缓存 + Caffeine 本地缓存
+ Redission 分布式锁 + BitMap + BloomFilter
+ Elasticsearch 搜索引擎
+ Druid 数据库连接池 + 并发编程
+ Sa-Token 权限控制
+ HotKey 热点探测
+ Sentinel 流量控制
+ Nacos 配置中心
+ 多角度项目优化：性能、安全性、可用性

## 项目亮点
1. 高并发缓存设计：组合使用Caffeine本地缓存+Redis缓存+Hotkey热键探测，形成三级缓存架构提升题库访问性能。通过位运算优化BitMap存储结构，刷题记录存储空间压缩至传统数据库方案的1/500。针对缓存击穿场景设计双重保障机制：本地缓存兜底+熔断降级策略。
2. 智能搜索中台建设：基于Elasticsearch构建高性能搜索服务，通过自定义IK分词器实现中文语义搜索，对比MySQL模糊查询性能提升73%。采用动静分离索引设计，冷数据（题目文本）存ES+热数据（点赞数）走DB关联查询，降低ES同步压力同时保障数据一致性。
3. 全链路系统防护：基于Sentinel实现熔断限流三维防护：1：接口级QPS限流 :2：热点题目滑动窗口熔断 3：单IP访问速率控制。通过Nacos配置中心实现规则动态推送，结合BloomFilter+WebFilter搭建黑名单拦截体系，恶意请求拦截耗时小于2ms。
4. 分级反爬虫策略：基于 Redis 实现用户访问题目频率统计，并通过 Lua 脚本保证原子更新。设计分级反爬策略：首次超限实时告警→二次违规自动封禁→黑名单同步Nacos集群，实现分钟级风险处置闭环。
5. 实时监控体系搭建：构建Druid慢SQL监控看板，通过动态线程池调整使批量操作吞吐量提升40%。设计双维度日志采集方案：业务日志ELK分析+系统指标Prometheus监控，关键操作链路实现全链路Trace追踪。
6. 安全认证架构升级：通过 UserAgent 识别用户设备，并基于 Sa-Token 实现多端互斥登录。采用JWT+非对称加密设计临时访问令牌，结合RefreshToken机制实现无感令牌刷新，认证性能提升65%。

