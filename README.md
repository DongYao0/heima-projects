# heima-projects

黑马程序员五个企业级项目的源码镜像，用于本地学习与调试。

> **这些代码不是我写的**。全部为第三方公开镜像，版权归原作者与黑马程序员（传智教育）所有，仅作个人学习研究使用，请勿商用或作为自己的作品发布。上游仓库见下表。

## 项目清单

| # | 项目 | 目录 | 技术栈 | 上游仓库 |
|---|---|---|---|---|
| 1 | 苍穹外卖 | [`sky-take-out/`](./sky-take-out) | Spring Boot 2.x + MyBatis + Redis + WebSocket + 微信支付 | [shuhongfan/sky-take-out](https://github.com/shuhongfan/sky-take-out) |
| 2 | 黑马点评 | [`hm-dianping/`](./hm-dianping) | Spring Boot + MyBatis-Plus + Redis（Lua 秒杀 / GEO / HyperLogLog） | [AZhangsan/heima-dianping-redis](https://github.com/AZhangsan/heima-dianping-redis) |
| 3 | 黑马头条 | [`heima-leadnews/`](./heima-leadnews) | Spring Cloud + Nacos + Kafka + MongoDB + ES + MinIO | [Money8888/heima-leadnews](https://github.com/Money8888/heima-leadnews) |
| 4 | 黑马商城 | [`hmall/`](./hmall) | Spring Cloud Alibaba + Docker + MQ + ES + Redis | [fangshuiyun/hmall](https://github.com/fangshuiyun/hmall) |
| 5 | 闪聚支付 | [`shanjupay/`](./shanjupay) | Spring Cloud + Nacos + Seata + RocketMQ + 微信支付 | [dong-ys/shanjupay](https://github.com/dong-ys/shanjupay) |

规划文档见 [`docs/黑马项目选型与双路线.md`](./docs/黑马项目选型与双路线.md)，详细结构说明见 [`sources/INDEX.md`](./sources/INDEX.md)。

## 目录结构

```
.
├── docs/
│   └── 黑马项目选型与双路线.md
└── sources/
    ├── INDEX.md
    ├── sky-take-out/      苍穹外卖（含管理端 Vue + 小程序 + PDF 讲义）
    ├── hm-dianping/       黑马点评
    ├── heima-leadnews/    黑马头条（仅后端）
    ├── hmall/             黑马商城（单体 + 微服务两版并存）
    └── shanjupay/         闪聚支付
```

## 建表脚本在哪

| 项目 | SQL |
|---|---|
| 苍穹外卖 | `sources/sky-take-out/sky.sql` |
| 黑马点评 | `sources/hm-dianping/src/main/resources/*.sql` |
| 黑马头条 | **缺失**，需从课程资料补 |
| 黑马商城 | **缺失**，需从课程资料补 |
| 闪聚支付 | `sources/shanjupay/config/sql/` |

## 运行前注意

- **版本不统一**：五个项目的 Spring Boot 跨 2.x 到 3.x 早期，依赖不要互相复制，`javax` / `jakarta` 会直接编译失败。
- **JDK**：苍穹外卖、黑马点评、黑马头条、闪聚支付建议 JDK 8/11；黑马商城建议 JDK 17。
- **黑马商城**的 `hm-service`（单体）与 `item-service`/`cart-service`（微服务）是同一业务的两套实现，不是两个项目。
- 中间件依赖较重：Nacos、Redis、MySQL、Kafka、RocketMQ、ES、MinIO、MongoDB，按需启动，不必全部拉起来。

## License

除黑马点评上游为 Apache-2.0 外，其余四个上游仓库**未声明许可证**，默认版权保留。本仓库仅做学习用途的镜像整理。
