# KOL 运营管理系统 PRD v1.0

## 1. 项目背景
当前团队使用 Excel 管理 KOL 资源、合作执行、产品分发、订单统计与返款流程。现有表格已经覆盖核心业务，但存在以下问题：

- 博主主档、合作记录、出单统计分散在多个 sheet，难以统一追踪
- 数据靠人工填写，字段格式不统一，统计口径不稳定
- 无法快速查看核心运营指标
- 无法自动跟踪带货链接出单情况
- 无法有效识别二次转化与高价值博主
- 执行链路中待跟进事项不透明，如签收未发帖、已发帖未录单、已出单未返款

本项目目标是用一个轻量 Web 系统替换现有 Excel，形成一个可持续使用的 KOL CRM + 合作流程管理 + 出单归因 + 数据看板系统。

## 2. 项目目标

### 2.1 核心目标
系统首页需要一眼看到：
- 博主库总人数
- 每周新增博主数量
- 每周新增合作数量
- 每周二次转化数量
- 每种合作类目的出单情况
- 带货合作当前执行情况
- 高表现博主与高表现产品

### 2.2 自动化目标
- 记录并管理 KOL 全生命周期
- 自动计算各类合作的出单表现
- 自动识别二次转化
- 支持 Amazon 带货链接对应订单数据的自动同步或半自动归因
- 自动提醒异常与待跟进合作

## 3. 用户角色

### 3.1 运营
- 录入博主信息
- 发起合作
- 跟进发货/签收/发帖
- 查看出单与返款状态
- 查看看板

### 3.2 管理者
- 查看团队整体合作表现
- 查看周/月趋势
- 查看不同合作类型的 ROI/出单表现
- 识别高价值博主与低效合作

## 4. 业务对象定义

### 4.1 博主
系统中的 KOL 主实体。一个博主可以有多次合作记录。

### 4.2 合作记录
一次合作行为，统一用一张合作表管理，按合作类型区分。

合作类型：
- SALES：带货
- GIVEAWAY：抽奖
- REVIEW：测评

### 4.3 订单记录
由 Amazon 数据源或人工导入而来的订单/出单记录，可归因到某个博主、某次合作、某个产品。

### 4.4 二次转化
定义为：
同一个博主在第二次及以上合作中继续产生有效订单，记为一次二次转化。

## 5. MVP 范围

### 5.1 必做
- 博主库管理
- 合作记录管理
- 产品库管理
- 订单导入与归因
- 首页看板
- 基础异常提醒
- Amazon 订单数据同步第一版

### 5.2 Amazon 数据同步策略
首版建议采用两阶段方案：

#### Phase 1
支持以下任一方式：
- 手工上传 Amazon 导出 CSV
- 运营粘贴订单数据
- 通过内部脚本定时同步已有报表

#### Phase 2
支持配置自动同步源：
- Amazon 官方报表/API
- 内部归因报表
- 统一订单同步任务

## 6. 功能需求

### 6.1 首页看板 Dashboard
顶部 KPI：
- 博主库总人数
- 本周新增博主数
- 本周新增合作数
- 本周有效出单数
- 本周二次转化数
- 本月转化率

图表模块：
1. 每周新增博主趋势
2. 每周新增合作趋势
3. 各合作类型出单对比
4. 各产品出单对比
5. Top 博主排行
6. 待跟进合作分布

待办列表：
- 已发货未签收
- 已签收未发帖
- 已发帖未录单
- 已出单未返款
- Giveaway 待上传结果

### 6.2 博主库模块
- 新增博主
- 编辑博主资料
- 搜索/筛选博主
- 查看博主历史合作
- 查看博主累计出单
- 查看是否为高价值博主
- 查看是否产生二次转化

### 6.3 合作记录模块
- 新建合作记录
- 选择合作类型
- 关联博主
- 关联产品
- 记录发货/签收/发帖/返款状态
- 支持上传帖子链接、订单号、返款凭证
- 支持状态筛选与批量更新

统一合作流程状态：
- DRAFT
- CONTACTED
- CONFIRMED
- PRODUCT_SELECTED
- ORDER_PLACED
- DELIVERED
- POST_PENDING
- POSTED
- ORDER_PENDING
- CONVERTED
- REFUNDED
- CLOSED
- CANCELLED

### 6.4 产品库模块
- 维护产品信息
- 支持按国家/类目/适用渠道筛选
- 与合作记录关联
- 与订单归因关联
- 看产品维度出单表现

### 6.5 订单模块
- 导入订单
- 自动归因到博主/合作/产品
- 查看订单明细
- 查看归因失败订单
- 支持人工修正归因
- 自动计算有效订单量、GMV、二转

归因逻辑优先级：
1. promo code
2. tracking code
3. amazon link id
4. 合作记录 + 产品 + 日期区间
5. 人工确认

### 6.6 提醒与异常模块
系统自动识别以下异常：
- 发货后超过 N 天未签收
- 签收后超过 N 天未发帖
- 发帖后超过 N 天未录订单
- 已出单但未返款
- Giveaway 开奖日已到但未更新状态

## 7. 数据表结构

### 7.1 influencers
- id
- name
- platform
- country
- follower_band
- follower_count
- social_url
- payment_method
- payment_account
- tags
- notes
- source_type
- status
- first_collab_at
- last_collab_at
- created_at
- updated_at

### 7.2 products
- id
- name_cn
- name_en
- code
- gender
- applicable_channel
- country
- product_url
- shop_name
- image_url
- status
- created_at
- updated_at

### 7.3 collaborations
- id
- collab_no
- influencer_id
- type
- status
- title
- planned_post_at
- posted_at
- delivered_at
- published_post_url
- logistics_status
- reward_status
- refund_status
- refund_proof_url
- post_content_type
- notes
- created_by
- created_at
- updated_at

### 7.4 collaboration_products
- id
- collaboration_id
- product_id
- quantity
- role
- created_at

### 7.5 orders
- id
- order_no
- order_date
- product_id
- quantity
- gmv
- currency
- source
- source_ref
- promo_code
- tracking_code
- amazon_link_id
- influencer_id
- collaboration_id
- is_refunded
- refund_status
- attribution_status
- created_at
- updated_at

## 8. 页面原型说明

### 8.1 首页 Dashboard
- 顶部筛选条：日期范围、国家、平台、合作类型、产品
- KPI 卡片：总博主数、本周新增博主、本周新增合作、本周有效出单、本周二次转化、本月转化率
- 图表：每周新增博主趋势、每周新增合作趋势、合作类型出单、产品出单
- 榜单：Top 博主、Top 产品
- 待办区：待发帖、待录订单、待返款、异常合作

### 8.2 博主列表页
字段：
- 博主名
- 平台
- 国家
- 粉丝数
- 合作次数
- 带货次数
- Giveaway 次数
- 测评次数
- 累计订单量
- 最近合作时间
- 是否二转
- 标签
- 状态

### 8.3 博主详情页
- 基本信息
- 指标区
- 合作记录区
- 订单记录区

### 8.4 合作列表页
字段：
- 合作编号
- 博主
- 合作类型
- 产品
- 当前状态
- 发货状态
- 签收时间
- 发帖时间
- 订单量
- 返款状态
- 创建时间

### 8.5 合作详情页
- 顶部信息
- 流程时间线
- 产品区
- 类型扩展信息区
- 附件区

### 8.6 订单列表页
字段：
- 订单号
- 日期
- 产品
- 数量
- GMV
- 来源
- 归因博主
- 归因合作
- 归因状态
- 退款状态

### 8.7 产品库页
字段：
- 产品名
- 英文名
- Code
- 性别向
- 国家
- 适用渠道
- 产品链接
- 当前合作次数
- 累计订单量
- 状态

## 9. 开发优先级

### P0
- 数据库建模
- 博主模块
- 合作模块
- 产品模块
- 订单导入
- Dashboard 首页

### P1
- 自动归因
- 异常池
- 待办提醒
- 博主详情页累计指标

### P2
- Amazon 自动同步
- 指标快照
- 高价值博主推荐

## 10. 验收标准
系统首版上线后，应满足：
1. 能导入现有 Excel 基础数据
2. 能新增/编辑博主
3. 能创建三类合作
4. 能关联产品与博主
5. 能导入订单并完成基础归因
6. 首页可展示核心 KPI 和趋势图
7. 能识别二次转化
8. 能看到待跟进合作清单
9. 能替代现有 Excel 做日常运营
