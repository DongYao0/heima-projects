# 黑马五项目源码清单

拉取时间：2026-10-23　｜　方式：`git clone --depth 1`（浅克隆，只取最新一次提交）
根目录：`E:\ai-project\hema_project\sources\`

| # | 项目 | 本地目录 | 上游仓库 | ★ | 语言 | 大小 | Java 文件 | 模块数 |
|---|---|---|---|---|---|---|---|---|
| 1 | 苍穹外卖 | `sky-take-out/` | [shuhongfan/sky-take-out](https://github.com/shuhongfan/sky-take-out) | 1528 | Java + Vue | 186M | 412 | 14 |
| 2 | 黑马点评 | `hm-dianping/` | [AZhangsan/heima-dianping-redis](https://github.com/AZhangsan/heima-dianping-redis) | 55 | Java | 26M | 80 | 1 |
| 3 | 黑马头条 | `heima-leadnews/` | [Money8888/heima-leadnews](https://github.com/Money8888/heima-leadnews) | 41 | Java | 5.8M | 384 | 13 |
| 4 | 黑马商城 | `hmall/` | [fangshuiyun/hmall](https://github.com/fangshuiyun/hmall) | 94 | Java + Vue | 64M | 176 | 10 |
| 5 | 闪聚支付 | `shanjupay/` | [dong-ys/shanjupay](https://github.com/dong-ys/shanjupay) | 20 | Java | 2.0M | 275 | 21 |

合计约 **284MB**、**1327 个 Java 文件**。

---

## 1. 苍穹外卖 `sky-take-out/`

上游是全套课程包，已做瘦身（原 347M → 现 186M）：

```
sky-take-out/
├── sky-take-out/               ← 真正的后端工程（多模块 Maven，569K）
│   ├── sky-common/             公共模块（常量、工具、异常）
│   ├── sky-pojo/               实体 / DTO / VO
│   └── sky-server/             服务端（controller / service / mapper）
├── sky.sql                     建表脚本（已从 资料/ 里拷出来）
├── project-rjwm-admin-vue-ts/  管理端前端（Vue3 + TS，6.4M）
├── mp-weixin/                  用户端微信小程序（2.8M）
├── springcache-demo/           缓存演示工程
├── 讲义/                        47M PDF（保留）
└── 必读：课程结构和能力目标.txt
```

**已清理**：`资料/`（104M，含安装包与素材）、`PPT/`（31M）、`nginx-1.20.2/`（21M）、`assets/`（5.6M）。
清理前已把 `资料/day01/数据库/sky.sql` 复制到根目录 `sky.sql`（20K）。

**想恢复**：这些文件仍被 git 跟踪，在 `sources/sky-take-out` 下执行 `git checkout -- .` 即可全部还原。
**想彻底腾出空间**：确认不再需要后执行 `git gc --prune=now`，可再省约 130M（`.git` 里仍存着被删文件的 blob），但之后就**无法还原**了。

- 技术栈：Spring Boot 2.x + MyBatis + Redis + WebSocket + 微信支付
- 数据库脚本：根目录 `sky.sql`；`springcache-demo/springcachedemo.sql` 是缓存演示用的独立库

## 2. 黑马点评 `hm-dianping/`

单模块 Maven 工程，结构最干净：

```
hm-dianping/
├── src/main/java/      controller / service / mapper / utils / config / dto
├── src/main/resources/ application.yaml、4 个 lua 脚本、sql
├── jmx/                JMeter 压测计划（缓存击穿.jmx）
└── images/
```

- 技术栈：Spring Boot + MyBatis-Plus + Redis（Lettuce）
- 亮点：`src/main/resources/` 下有 `seckill.lua` / `seckill2.lua` / `unlock.lua` / `unlock2.lua`——带 `2` 的是改进版（含库存判断/逻辑过期那套），对比着看最值
- `jmx/缓存击穿.jmx` 是现成的压测计划，可直接导入 JMeter
- 唯一带 LICENSE 的仓库（Apache-2.0）

## 3. 黑马头条 `heima-leadnews/`

完整多模块微服务后端，模块划分和课程一致：

```
heima-leadnews-apis        Feign 接口 + 微服务间 DTO
heima-leadnews-common      公共依赖（Nacos/Kafka/MinIO 封装）
heima-leadnews-model       实体与 POJO
heima-leadnews-utils
heima-leadnews-admin       后台管理
heima-leadnews-article     文章（内容生产、审核）
heima-leadnews-behavior    行为（点赞/阅读/收藏）
heima-leadnews-crawler     爬虫
heima-leadnews-login       登录（自媒体端）
heima-leadnews-media       素材管理
heima-leadnews-user        用户（APP 端）
```

- 技术栈：Spring Cloud + Nacos + Kafka + MongoDB + MinIO + Elasticsearch
- **缺 SQL**：仓库里没有 `.sql`，表结构要另找课程资料
- **无前端**：只有后端（自媒体端/APP 端前端不在这个仓库）
- 最后提交 2020-09，对应的是 Kafka/Nacos 那一版课程

## 4. 黑马商城 `hmall/`

单体 → 微服务拆分的完整形态，两个阶段都在：

```
hmall/
├── hm-service/        单体版（第一阶段的完整业务）
├── hm-common/         公共模块
├── hm-api/            Feign 客户端 + DTO
├── hm-gateway/        网关
├── item-service/      商品服务（拆分后）
├── cart-service/      购物车服务
├── pay-service/       支付服务
├── hmall-nginx/       nginx + 前端静态资源
└── sentinel-dashboard.jar
```

- 技术栈：Spring Cloud Alibaba（Nacos / OpenFeign / Gateway / Sentinel）+ Docker + MQ + ES + Redis
- **注意**：`hm-service`（单体）和 `item-service`/`cart-service`（微服务）是**同一业务的两套实现**，不要以为是两个项目
- **缺 SQL**：同样没有 `.sql` 文件
- 含 `sentinel-dashboard.jar`，可省去自己下载

## 5. 闪聚支付 `shanjupay/`

模块最全，聚合支付链路完整：

```
shanjupay-common               公共
shanjupay-gateway              网关
shanjupay-uaa                  认证授权（OAuth2）
shanjupay-merchant             商户服务
shanjupay-merchant-application 商户应用（SaaS 端）
shanjupay-transaction          交易服务（核心：支付订单）
shanjupay-user                 用户服务
shanjupay-payment-agent        支付代理（对接微信/支付宝）
sailing/                       基础工程（课程起步代码）
config/                        配置（Nacos 等）
```

- 技术栈：Spring Cloud + Nacos + Seata + RocketMQ + 微信支付
- 有 2 个 `.sql`：`config/sql/shanjupay-init.sql`、`config/sql/shanjupay_saas.sql`，可直接建库
- 最后提交 2021-07，Spring Boot 版本较老（2.x 早期），JDK 8 环境更稳

---

## 使用前必读

1. **全部是第三方镜像，不是官方仓库**。官方代码只随课程发放，GitHub 上这些都是学员自己上传的。
2. **许可证**：只有黑马点评带 Apache-2.0；其余四个**无 LICENSE**，即默认版权保留。自己学习没问题，但**不要直接当自己的项目发布或商用**。
3. **浅克隆**：`--depth 1`，没有历史提交。需要完整历史执行 `git fetch --unshallow`。
4. **版本差异**：这五个仓库的 Spring Boot 版本跨 2.x 到 3.x 早期，互相抄依赖会踩 `javax` / `jakarta` 的坑，各项目独立看。
5. **SQL 缺失**：黑马头条、黑马商城没有建表脚本，需要从课程资料补。

## 目录结构

```
E:\ai-project\hema_project\
├── docs\
│   └── 黑马项目选型与双路线.md
└── sources\
    ├── INDEX.md          ← 本文件
    ├── sky-take-out\     苍穹外卖
    ├── hm-dianping\      黑马点评
    ├── heima-leadnews\   黑马头条
    ├── hmall\            黑马商城
    └── shanjupay\        闪聚支付
```
