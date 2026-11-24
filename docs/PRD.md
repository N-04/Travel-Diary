# 旅行足迹 Travel Diary — 产品需求文档（PRD）

> 以「地图解锁」为核心的个人旅行足迹记录工具

| 项目 | 内容 |
| --- | --- |
| 文档名称 | 旅行足迹 Travel Diary 产品需求文档 |
| 文档版本 | v1.0 |
| 文档状态 | 待评审（Draft for Review） |
| 撰写日期 | 2026-11-24 |
| 目标仓库 | https://github.com/N-04/Travel-Diary |
| 适用阶段 | MVP 设计 → 研发实现 → 验收 |
| 评审人 | 待定 |

### 修订记录

| 版本 | 日期 | 修订人 | 说明 |
| --- | --- | --- | --- |
| v1.0 | 2026-11-24 | — | 首版：明确产品定位、解锁口径、功能范围与技术选型 |

### 目录

1. [背景与问题](#1-背景与问题)
2. [目标用户与使用场景](#2-目标用户与使用场景)
3. [目标与非目标](#3-目标与非目标)
4. [核心概念与信息架构](#4-核心概念与信息架构)
5. [关键流程](#5-关键流程)
6. [功能需求](#6-功能需求)
7. [页面与交互](#7-页面与交互)
8. [技术方案](#8-技术方案)
9. [非功能需求](#9-非功能需求)
10. [数据分析与指标](#10-数据分析与指标)
11. [里程碑与排期](#11-里程碑与排期)
12. [验收标准](#12-验收标准given--when--then)
13. [风险与待确认问题](#13-风险与待确认问题)
14. [附录](#14-附录)

---

## 1. 背景与问题

### 1.1 现状与痛点

旅行记录类产品长期存在一个断层：**「去过哪里」这件事，要么依赖定位常驻，要么依赖照片被动推断，没有一个工具真正把「足迹」当作可管理的资产。**

| # | 现状 | 用户痛点 | 现有替代方案的缺陷 |
| --- | --- | --- | --- |
| P1 | 高德/百度/Google 的足迹、时间线功能 | 只有在运行地图 App 且授予后台定位时才会打点；关掉就漏记 | 耗电、隐私顾虑大；补录困难；以「轨迹」为单位，没有「解锁了哪些省/市」的视角 |
| P2 | 相册地图、照片时间线 | 只能导入拍过照的地点 | 没拍照的行程（中转、路过、出差当天往返）无法记录；无法标记交通方式；照片分散在各设备 |
| P3 | 航旅纵横等飞行记录工具 | 只覆盖民航单一方式 | 高铁、自驾、徒步、骑行、轮船等出行方式无法记录；只到机场城市，不到省/市层级 |
| P4 | Notion / Excel / 手账 | 纯文本，无地图，无进度 | 无法可视化、无法统计「解锁百分比」、维护成本高、容易放弃 |
| P5 | 社交类足迹产品 | 偏打卡分享 | 数据归属不清、隐私不可控、进度体系弱 |

### 1.2 机会

把「旅行足迹」抽象为**可管理的记录资产 + 可量化的解锁进度**：

1. **双录入通道**：手动录入解决「没拍照也能记」，照片导入解决「批量补录历史」；
2. **行政区口径的进度体系**：国家 → 省/州 → 城市三级解锁，百分比可解释、可比较；
3. **出行方式是一等公民**：每条记录带交通方式，连线的形状与颜色随方式变化，像航旅纵横的航线图，但覆盖全部出行方式；
4. **地图光点 + 连线**：以时间顺序把足迹串成一条「旅行光路」，直观看到自己解锁的地图。

### 1.3 一句话定位

> 一个以「地图解锁进度」为核心、支持手动与照片双通道录入、按国家/省/市分级统计的个人旅行足迹管理工具。

---

## 2. 目标用户与使用场景

### 2.1 目标用户

| 画像 | 特征 | 核心诉求 |
| --- | --- | --- |
| A. 旅行爱好者 | 每年 3～10 次出行，喜欢看「集齐」的成就感 | 记录到访城市、看解锁百分比、回看足迹地图 |
| B. 出差常客 | 高频、短停留、常中转，行程碎片化 | 快速补录（照片批量导入）、按年查看、区分「停留/途经」 |
| C. 地图收集控 | 在意「去过多少省/多少国」 | 明确的统计口径、精确的百分比、可分享的成就卡 |
| D. 隐私敏感型记录者 | 不愿开后台定位、不愿上传原图 | 全手动可控录入、本地部署、原图可丢弃 |

### 2.2 核心用户故事

| ID | 用户故事 | 优先级 |
| --- | --- | --- |
| US-01 | 作为旅行者，我想在旅行结束后手动补录一次到访（地点、时间、交通方式），以便不漏记 | P0 |
| US-02 | 作为出差常客，我想把一批带 GPS 的照片导入，自动生成候选记录，确认后批量入账 | P0 |
| US-03 | 作为收集控，我想在地图上看到已解锁国家/省/市的高亮与百分比，以便知道还差多少 | P0 |
| US-04 | 作为旅行者，我想按「国家 → 省/市」浏览到访记录与次数，像航旅纵横看飞行记录一样 | P0 |
| US-05 | 作为用户，我想把两个地点之间标记为「飞机/高铁/自驾」，让连线体现出行方式 | P0 |
| US-06 | 作为用户，我想区分「停留」与「途经」，避免中转城市虚增解锁数 | P0 |
| US-07 | 作为用户，我想按年份/交通方式筛选足迹，查看某一年去了哪里 | P1 |
| US-08 | 作为用户，我想导出全部数据，避免被平台锁定 | P1 |
| US-09 | 作为用户，我想分享一张解锁成就卡，且不泄露精确坐标 | P2 |

### 2.3 典型场景

- **场景 1｜周末自驾**：周六开车去邻市古镇，App 里手填「杭州 → 湖州·南浔」，交通方式「自驾」，上传 3 张照片，地图上多一个光点、一条实线。
- **场景 2｜跨省出差**：3 天跑 2 个省 3 个市，其中 1 个城市只是高铁中转。录入时把中转城市标记为「途经」，进度只加 2 城。
- **场景 3｜历史补录**：导入 2019 年日本旅行的 200 张照片，系统按时间 + 距离聚类为 6 组候选（东京、镰仓、箱根…），确认后一次生成 6 条记录。
- **场景 4｜年终回顾**：打开地图，看到「国家 12 / 195 · 6.2%」，中国点亮 9 省 31 市，点开某个省份看城市清单与到访次数。

---

## 3. 目标与非目标

### 3.1 产品目标（可度量）

| 目标 | 度量方式 | MVP 目标值 |
| --- | --- | --- |
| 记录成本低 | 手动新建一条记录 ≤ 60 秒 | 中位数 ≤ 45s |
| 补录效率高 | 100 张照片导入到确认完成 | ≤ 3 分钟（异步处理） |
| 进度可信 | 解锁口径可解释、无重复计数 | 口径文档化 + 单测覆盖 |
| 可视化有记忆点 | 地图页停留时长、进度页访问率 | 地图页停留 ≥ 90s |
| 数据可带走 | 一键导出 JSON/CSV | P1 交付 |

### 3.2 版本范围

| 范围 | 内容 |
| --- | --- |
| **MVP（P0）** | 账户（单用户/邮箱登录）、手动录入、照片导入（含 EXIF 定位与聚类确认）、地图光点与连线、国家/省/市三级浏览、解锁进度与百分比、出行方式、停留/途经区分、Redis 缓存 |
| **P1** | 搜索筛选、年度统计、数据导出、地图时间轴回放、移动端 PWA、多用户与分享链接 |
| **P2** | 成就徽章与等级、年度回顾报告、社交排行榜、离线行政区判定（完全本地逆地理编码）、多语言 |

### 3.3 非目标（本期明确不做）

1. **不做实时 GPS 轨迹记录/后台定位**——这是本产品与地图 App 的根本差异，避免耗电与隐私争议；
2. **不做原生移动 App**（响应式 Web + PWA 即可）；
3. **不做机票/12306 自动同步**——账号体系与合规成本高；
4. **不做社交关系链与排行榜**（P2 再议）；
5. **不做 UGC 攻略/游记内容平台**——只做个人记录；
6. **不做商业化（广告/会员）**。

---

## 4. 核心概念与信息架构

### 4.1 领域概念

| 概念 | 定义 | 说明 |
| --- | --- | --- |
| User | 用户 | MVP 单用户优先，但所有表预留 `user_id`，为多用户留扩展位 |
| Trip | 行程 | 一次出行的聚合（可选），如「2026 国庆·川西环线」 |
| Visit | 到访记录 | **最小原子单位**：某人在某时到访某地 |
| Place | 地点 | 名称 + 坐标 + 行政区归属（国家/一级行政区/城市） |
| AdminDivision | 行政区 | 三级：国家（level 0）、一级行政区（level 1，省/州/都道府县）、城市（level 2） |
| TransportMode | 出行方式 | 飞机、高铁、普速火车、长途汽车、自驾、骑行、徒步、轮船、其他 |
| VisitType | 到访类型 | `STAY`（停留）/ `TRANSIT`（途经，中转） |
| Unlock | 解锁 | 某行政区下存在至少一条计入统计的 `STAY` 到访 |
| Progress | 进度 | 解锁数 / 分母，见 §6.5 口径 |

### 4.2 关系模型

```mermaid
erDiagram
    User ||--o{ Trip : owns
    User ||--o{ Visit : records
    User ||--o{ Media : uploads
    Trip ||--o{ Visit : groups
    Visit }o--|| Place : at
    Visit ||--o{ Media : evidences
    Place }o--o| AdminDivision : city
    Place }o--o| AdminDivision : province
    Place }o--o| AdminDivision : country
    AdminDivision ||--o{ AdminDivision : parent
```

### 4.3 信息架构

```
旅行足迹
├── 地图（首页）            /                    光点·连线·解锁高亮·进度环
├── 国家与地区             /countries            国家卡片 + 各国进度
│   └── 国家详情           /countries/[code]     省/州清单 + 进度
│       └── 省/州详情      /regions/[code]       城市清单 + 到访次数
├── 时间线                 /timeline             按时间倒序的行程流
├── 记录
│   ├── 新建/编辑          /records/new, /records/[id]
│   ├── 照片导入           /import
│   └── 记录详情           /records/[id]
├── 统计（P1）             /stats
└── 设置                   /settings             账户·地图源·解锁口径·数据导出
```

---

## 5. 关键流程

### 5.1 手动录入

```mermaid
flowchart TD
    A[点击「记录足迹」] --> B[填写时间范围与标题]
    B --> C{地点来源}
    C -->|搜索行政区| D[级联选择 国家→省→市]
    C -->|地图拾取| E[点击地图取坐标]
    C -->|照片定位| F[选照片自动填]
    D --> G[可选：具体地点名/POI]
    E --> G
    F --> G
    G --> H[选择到访类型 停留/途经]
    H --> I[选择出行方式]
    I --> J[上传照片·备注·评分]
    J --> K{校验}
    K -->|通过| L[保存 Visit]
    K -->|失败| B
    L --> M[失效进度缓存·异步重算]
    M --> N[地图新增光点·提示解锁结果]
```

**校验规则**

- 必填：开始时间、地点（行政区或坐标至少其一）、到访类型；
- 结束时间 ≥ 开始时间；允许未来时间但需确认（防止误填）；
- 同一用户、同一城市、同一天已存在 `STAY` 记录时，提示「合并到已有记录 / 仍然新建」，默认合并并 `visit_count + 1`；
- 坐标精度保留 6 位小数；记录写入时同步落行政区冗余字段，避免后续行政区划变更影响历史统计。

### 5.2 照片导入

```mermaid
flowchart TD
    A[选择/拖入照片 ≤100 张] --> B[上传至服务端 临时区]
    B --> C[读取 EXIF DateTimeOriginal + GPS]
    C --> D{有 GPS?}
    D -->|否| E[标记「需手动定位」]
    D -->|是| F[坐标 → 行政区 逆地理]
    F --> G[按时间 12h + 距离 20km 聚类]
    G --> H[生成候选记录 列表]
    H --> I[用户逐条确认/合并/忽略/改行政区]
    I --> J[批量入库 Visit + Media]
    J --> K[清理临时文件·失效进度缓存]
    E --> I
```

**导入规则**

- 单张 ≤ 20 MB，批量 ≤ 100 张/次，格式 JPG/JPEG/PNG/HEIC/WEBP；
- 只读取 EXIF，**不上传原图到任何第三方**；逆地理编码仅发送坐标（P1 提供纯本地判定开关）；
- 照片去重：SHA-256（原图）唯一约束，重复导入直接跳过并提示；
- 无 GPS 的照片按拍摄时间排序，允许用户手动指定行政区，一次指定应用到同批次；
- 拍摄时间按「拍摄地 UTC 偏移」换算展示，避免跨时区错日；
- 导入任务异步执行，前端轮询任务状态（Redis 存进度），失败可重试单张。

### 5.3 进度计算与缓存失效

```mermaid
sequenceDiagram
    participant U as 用户
    participant W as Next.js API
    participant DB as PostgreSQL
    participant R as Redis
    U->>W: GET /api/progress
    W->>R: GET progress:{userId}:v1
    alt 命中
        R-->>W: 进度 JSON
        W-->>U: 秒回
    else 未命中
        W->>DB: 聚合查询（城市/省/国家 去重计数）
        DB-->>W: 计数结果
        W->>R: SETEX progress:{userId}:v1 3600 + 结果
        W-->>U: 返回
    end
    Note over U,R: 任意 Visit 写操作后 DEL progress:{userId}:v1
```

> 规模假设：个人用户 ≤ 10,000 条 Visit，单次聚合 < 100ms，可直接实时计算 + 缓存；多用户规模化后再引入 `progress_snapshot` 物化表与异步重算。

---

## 6. 功能需求

> 优先级定义：**P0 = MVP 必须**；P1 = 次版本；P2 = 远期。

### 6.1 账户与设置

| 编号 | 需求 | 优先级 | 规则与说明 |
| --- | --- | --- | --- |
| FR-1.1 | 账户体系 | P0 | 首版支持「单用户本地模式」（无需注册）与「邮箱 + 密码登录」两种形态，由环境变量 `AUTH_MODE` 切换；密码 bcrypt 存储，会话用 HttpOnly Cookie |
| FR-1.2 | 数据隔离 | P0 | 所有业务表带 `user_id`，API 层强制按当前会话过滤，禁止越权查询 |
| FR-1.3 | 个人设置 | P1 | 地图源、国家分母口径（193/195/197/249）、是否将「途经」计入解锁（默认关）、首页默认视图 |
| FR-1.4 | 数据导出/导入 | P1 | 导出 JSON（全量）与 CSV（记录清单）；导入 JSON 用于迁移与备份 |
| FR-1.5 | 注销与清空 | P1 | 二次确认 + 输入验证词；级联删除 Visit/Media/文件 |

### 6.2 手动录入（P0）

**字段设计**

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| 标题 | string(60) | 否 | 默认「城市名 · 日期」，可改 |
| 开始时间 | datetime | 是 | 精确到分钟；默认当前时间 |
| 结束时间 | datetime | 否 | 支持跨天；为空视为当天 |
| 国家 | FK | 是* | 级联选择器第 1 级，支持中英文/拼音搜索 |
| 一级行政区 | FK | 否 | 级联第 2 级；缺失时按坐标自动匹配 |
| 城市 | FK | 否 | 级联第 3 级；与坐标至少有一个 |
| 地点名称 | string(80) | 否 | 如「西湖·断桥」，自由文本 |
| 坐标 | lat/lng | 否 | 地图拾取或手填；与城市至少有一个 |
| 到访类型 | enum | 是 | 停留 / 途经（默认停留） |
| 出行方式 | enum | 否 | 见 §6.8；单点记录可为空 |
| 出发地 | FK/坐标 | 否 | 用于在两点之间画连线；缺省时按时间顺序连接 |
| 同行人 | string(120) | 否 | 自由文本 |
| 照片 | Media[] | 否 | ≤ 20 张/条 |
| 备注 | text(2000) | 否 | 支持 Markdown 子集（换行、链接） |
| 评分 | 1–5 | 否 | 用于「最爱的目的地」统计 |

> \* 仅填写坐标时，「国家/省/市」由系统按坐标自动归属并回填，用户可手动修正；行政区与坐标至少要有一项，否则无法统计解锁。

**交互要求**

- 级联选择器支持键盘操作与拼音首字母检索；
- 地图拾取坐标后自动反查行政区并回填，用户可手动覆盖；
- 保存后不跳页，地图上高亮新增点并提示「🎉 新解锁：浙江省 · 第 3 城」；
- 表单草稿自动存 localStorage，防止误关丢失。

### 6.3 照片导入（P0）

| 编号 | 需求 | 优先级 | 说明 |
| --- | --- | --- | --- |
| FR-3.1 | 批量上传 | P0 | 拖拽/选择文件；上传进度条；单张失败不影响其他 |
| FR-3.2 | EXIF 解析 | P0 | 读取 `DateTimeOriginal`、`GPSLatitude/Longitude/Altitude`、`Orientation`；无 EXIF 时回退文件修改时间并标注「时间来自文件系统」 |
| FR-3.3 | 逆地理编码 | P0 | 坐标 → 国家/省/市；优先查本地行政区边界（PostGIS `ST_Intersects`），未命中调用在线服务并缓存 30 天 |
| FR-3.4 | 智能聚类 | P0 | 时间窗 12h（可调 1–48h）+ 距离 20km（可调 1–200km）；同簇生成一条候选，取簇内停留最久的点作为主地点 |
| FR-3.5 | 候选确认 | P0 | 列表展示：缩略图、时间、行政区、置信度；支持逐条改行政区/改类型/合并/忽略 |
| FR-3.6 | 去重 | P0 | 原图 SHA-256 唯一；`(user, 城市, 日期)` 已存在时提示合并 |
| FR-3.7 | 缩略图 | P0 | 服务端生成 320px WebP 缩略图与 1600px 预览图；列表只加载缩略图 |
| FR-3.8 | 隐私 | P0 | EXIF 不随 API 返回；分享/导出默认剔除精确坐标；原图保留策略可配置（保留/仅缩略图/30 天清理） |
| FR-3.9 | 无 GPS 处理 | P0 | 单独分组「待定位」，支持批量指定一个行政区后一次性入账 |
| FR-3.10 | 任务状态 | P0 | Redis 存 job 状态（queued/processing/succeeded/failed + 进度百分比），前端 1s 轮询 |

**性能约束**：100 张照片（总 500 MB 内）从上传到生成候选 ≤ 3 分钟；解析并发 4，超出排队。

### 6.4 地图可视化（P0）

| 编号 | 需求 | 优先级 | 说明 |
| --- | --- | --- | --- |
| FR-4.1 | 底图 | P0 | MapLibre GL JS + 可配置瓦片源（天地图/高德栅格/MapTiler/OSM）；默认随部署区域切换，避免国内不可用 |
| FR-4.2 | 光点层 | P0 | 每个已解锁城市一个发光点；半径 4–12px 随到访次数增长；停留为实心光晕，途经为空心小点 |
| FR-4.3 | 连线层 | P0 | 按开始时间排序连接相邻到访；飞机走大圆弧线（虚线），火车/自驾为实线并按方式着色，徒步为点线；跨 180° 经线自动拆分线段 |
| FR-4.4 | 行政区高亮 | P0 | 已解锁一级行政区半透明填充（按国家分组上色），已解锁国家在低 zoom 下高亮 |
| FR-4.5 | 进度浮层 | P0 | 左上角进度环：国家 6.2% / 省 24 个 / 城市 87 个；点击展开明细 |
| FR-4.6 | 交互 | P0 | 点击光点弹出记录卡片（缩略图 + 日期 + 方式 + 编辑入口）；滚轮缩放、双击放大、移动端双指 |
| FR-4.7 | 图层控制 | P0 | 可独立开关 光点/连线/行政区/途经点 |
| FR-4.8 | 筛选 | P1 | 年份、交通方式、国家；筛选时连线与进度同步变化 |
| FR-4.9 | 时间轴回放 | P1 | 播放按钮按时间逐条点亮，速度 1×/2×/4× |
| FR-4.10 | 点聚合 | P1 | > 2,000 点时按 zoom 聚合（Supercluster），点击展开 |
| FR-4.11 | 分享卡 | P2 | 生成 1080×1440 图片（进度 + 地图轮廓 + 城市数），无精确坐标 |

**光点连线视觉规范**

- 光点：主色 `#22D3EE` 光晕 + 白色圆心，到访 ≥ 3 次升级为金色 `#F59E0B`；
- 连线：飞机 `#6366F1` 虚线弧；高铁 `#22C55E`；普速火车 `#14B8A6`；自驾 `#F97316`；骑行 `#A855F7`；徒步 `#EAB308` 点线；轮船 `#0EA5E9` 波浪虚线；其他 `#94A3B8`；
- 深色底图为默认主题，Bootstrap 5 的 `data-bs-theme="dark"` 承载其余 UI。

### 6.5 解锁进度与百分比（核心）

#### 6.5.1 统计口径（必须唯一、可解释）

设：

- `U_country` = 已解锁国家集合 = 存在 ≥1 条 `visitType = STAY` 且计入统计的 Visit 的国家；
- `U_prov(c)` = 国家 `c` 内已解锁一级行政区集合；
- `U_city(p)` = 一级行政区 `p` 内已解锁城市集合；
- `N_country` = 国家分母，默认 **195**（设置项：193 联合国会员国 / 195 含观察员 / 197 含科索沃等 / 249 ISO 全部地区）；
- `N_prov(c)` = 该国家一级行政区总数（仅有行政区数据的国家参与计算）；
- `N_city(p)` = 该一级行政区内城市/县总数（仅中国大陆、美国、日本等有完整三级数据的地区参与）。

**计算公式**

```
国家解锁率   R_country      = |U_country| / N_country
省级解锁率   R_prov(c)      = |U_prov(c)| / N_prov(c)
城市解锁率   R_city(p)      = |U_city(p)| / N_city(p)
综合探索度   D = 0.5 × R_country
                 + 0.3 × avg(R_prov(c) over c ∈ U_country)
                 + 0.2 × avg(R_city(p) over p ∈ U_prov)
```

- 权重、分母可在设置中调整；某层级无数据（如分母为 0）时剔除该层并按剩余权重归一化；
- 百分比保留 1 位小数；`< 0.1%` 显示「<0.1%」；未解锁显示 0%；
- **探索值**（用于等级与成就）：`P = 10 × |U_city| + 30 × |U_prov| + 100 × |U_country|`；
- **计数规则**：`TRANSIT` 默认不计入任何解锁口径（设置可开）；同一城市多天多次到访只计 1 个解锁城市，但累计 `visit_count`；删除记录后进度实时回退。

#### 6.5.2 展示规则

| 位置 | 展示内容 |
| --- | --- |
| 地图首页进度环 | 国家百分比（主）+ 省/市计数 |
| 国家列表卡片 | 每个国家的省份进度条、最近到访时间、城市数 |
| 国家详情页 | 已解锁省/州清单（高亮）+ 「N / M 省」 |
| 省/州详情页 | 城市清单（已解锁/未解锁两种态）+ 次数角标 |
| 记录保存反馈 | Toast：「新解锁：四川省 · 第 5 城」/「已解锁第 13 个国家」 |

#### 6.5.3 边界与异常

| 场景 | 处理 |
| --- | --- |
| 坐标落在海面/边界缝隙 | 就近归属：`ST_DWithin` 最近质心（≤ 50 km），仍失败则要求手动选择 |
| 国家无三级数据（如多数小国） | 只统计国家与一级行政区，城市级不展示百分比，仅展示「已解锁 N 城」 |
| 港澳台 | 按「中国」下的省级行政区统计（需在行政区数据中明确口径，见 Q4） |
| 跨境连线 | 连线保留，但两国解锁分别计算 |
| 行政区划调整 | 历史记录落库时固化行政区 ID 与快照名称，不受后续数据升级影响 |

### 6.6 国家 / 省 / 市分级浏览（P0）

| 编号 | 需求 | 优先级 | 说明 |
| --- | --- | --- | --- |
| FR-6.1 | 国家列表 | P0 | 卡片网格：国旗/名称、省份进度、城市数、最近到访；支持「已解锁 / 全部」切换与搜索 |
| FR-6.2 | 国家详情 | P0 | 地图轮廓 + 省份清单 + 该国内全部记录；按「最近到访 / 名称 / 进度」排序 |
| FR-6.3 | 省/州详情 | P0 | 城市清单（类似航旅纵横城市列表），显示每个城市到访次数与首末次时间 |
| FR-6.4 | 记录聚合 | P0 | 点击城市展开该城市的 Visit 列表，可编辑/删除 |
| FR-6.5 | 空态 | P0 | 未解锁国家显示「还没点亮」，并提供「去记录」入口 |

### 6.7 行程与记录详情（P0）

- **记录详情**：时间、地点（行政区全路径）、到访类型、交通方式、备注、照片墙（懒加载）、编辑/删除、相邻记录的前后导航；
- **行程（Trip）**：将多条 Visit 归入一次行程（如「2026 国庆·川西」），提供行程内时间轴与小地图；
- 删除需二次确认，删除后进度缓存失效。

### 6.8 出行方式（P0）

| 值 | 展示 | 连线路由 | 备注 |
| --- | --- | --- | --- |
| `PLANE` | ✈️ 飞机 | 大圆弧线 | 两点可跨洲；自动处理反经线 |
| `HSR` | 🚄 高铁/动车 | 直线 | |
| `TRAIN` | 🚂 普速火车 | 直线 | |
| `BUS` | 🚌 长途汽车 | 直线 | |
| `CAR` | 🚗 自驾 | 直线 | 可后续接入路线规划 |
| `BIKE` | 🚲 骑行 | 直线 | |
| `WALK` | 🥾 徒步 | 点线 | |
| `FERRY` | ⛴️ 轮船 | 直线（波浪虚线） | |
| `OTHER` | ➡️ 其他 | 直线 | |

规则：记录可无出行方式（仅一个光点）；两条记录之间有方式时按方式渲染连线，无方式时用默认灰色实线；同一段行程中方式可不同（去程飞机、返程高铁）。

### 6.9 搜索与筛选（P1）

- 关键词搜索：地点名、备注、城市名、国家名（支持拼音）；
- 筛选维度：年份、国家、交通方式、到访类型；
- 搜索结果高亮并在地图上联动定位。

### 6.10 统计与成就（P1 / P2）

- P1：年度概览（本年到访城市数、新增国家、最远距离、最常方式）、方式分布饼图、月度出行柱状图；
- P2：徽章（首个国家、解锁 10 城、单次跨 3 省…）、等级（探索值分段 L1–L8）、年度报告长图。

### 6.11 数据安全与隐私（P0）

1. 所有记录仅本人可见，默认无公开主页；
2. 分享能力（P2）只输出聚合数据与轮廓图，不输出坐标与照片；
3. 照片 EXIF 在服务端解析后剥离，API 不返回 GPS 原始标签；
4. 支持一键清空账户与本地文件；
5. 上传接口校验 MIME + 文件头魔数 + 扩展名白名单，拒绝 SVG/可执行文件；
6. 逆地理编码失败时**不阻塞**录入，用户可手选。

---

## 7. 页面与交互

### 7.1 路由表

| 路由 | 页面 | 优先级 |
| --- | --- | --- |
| `/` | 地图首页（光点 + 连线 + 进度环） | P0 |
| `/countries` | 国家与地区列表 | P0 |
| `/countries/[code]` | 国家详情（省/州清单） | P0 |
| `/regions/[code]` | 省/州详情（城市清单） | P0 |
| `/timeline` | 时间线 | P1 |
| `/records/new` | 新建记录 | P0 |
| `/records/[id]` | 记录详情 | P0 |
| `/import` | 照片导入 | P0 |
| `/stats` | 统计 | P1 |
| `/settings` | 设置 | P0（基础项） |

### 7.2 地图首页线框

```
+--------------------------------------------------------------+
| 旅行足迹        [地图] [国家] [时间线]        [+ 记录] [导入] |
+--------------------------------------------------------------+
|  进度环 6.2%                                                |
|  12 / 195 国家                                              |
|  ────────────────────────                                   |
|  省份 24    城市 87                                         |
|  ┌───────────────────────────────────────────────┐          |
|  │                                   ●─ ─ ─●      │  ← 飞机弧线 │
|  │        ●────●        ●                        │          |
|  │        │    │        │  ●──●                 │          |
|  │        ●────●        ●                        │          |
|  │                                               │  [图层]  │
|  └───────────────────────────────────────────────┘  [筛选]  │
+--------------------------------------------------------------+
```

### 7.3 交互与状态

- 首次进入：无数据时展示引导空态（「记录第一次足迹」+「导入照片」双按钮）；
- 加载：地图骨架屏 + 进度环占位，避免布局抖动；
- 移动端：底部 Tab（地图/国家/时间线/我的），表单全屏；
- 错误：网络失败保留草稿并提示重试；导入失败逐张列出原因；
- 无障碍：进度环与百分比提供文本可读标签；颜色不作为唯一方式标识（方式同时有图标与文字）。

---

## 8. 技术方案

### 8.1 技术栈总览

| 层 | 选型 | 理由 |
| --- | --- | --- |
| 前端框架 | Vue 3 + TypeScript + Vite | 组合式 API 与类型安全；构建快 |
| UI 框架 | Bootstrap 5.3（+ Bootstrap Icons） | 成熟、响应式栅格、可自定义主题；深色模式内置 |
| 状态管理 | Pinia + Vue Router | 轻量、TS 友好 |
| 地图 | MapLibre GL JS（+ 可选 MapTiler/天地图瓦片） | 开源无 key 依赖（瓦片可换），支持 GeoJSON 光点/连线/填充 |
| 图表 | ECharts（P1 统计） | 中文生态好、按需引入 |
| 后端 | Next.js（App Router）Route Handlers + Node.js 运行时，**纯 API 模式、不承载页面** | 与前端同仓同 TS 类型，API 即文件路由；Vue 页面由独立 SPA 承载 |
| 校验 | Zod | 请求体与响应 schema 复用，前后端共享类型 |
| ORM | Prisma | 迁移与类型生成；PostGIS 部分用 `$queryRaw` |
| 数据库 | PostgreSQL 16 + PostGIS 3.4（推荐，见 §8.4） | 点面判断、距离聚类、聚合统计一步到位 |
| 缓存 | Redis 7（ioredis） | 进度缓存、逆地理缓存、导入任务状态、限流 |
| 图片处理 | sharp + exifr（EXIF）+ heic-convert（HEIC 转码） | 服务端生成缩略图、提取元数据 |
| 对象存储 | 本地卷 / MinIO（S3 兼容） | 照片与缩略图 |
| 部署 | Docker Compose（spa+nginx / api / postgres / redis / minio） | 一键自托管；Vercel 方案见 §8.9 |

> **拓扑说明**：Next.js 在本项目中只作 API 层（`apps/api`，App Router 的 Route Handlers），不写 React 页面；Vue SPA（`apps/web`，Vite 构建）由 Nginx 托管静态资源，并把 `/api/*` 反代到 Next.js，保证同源、Cookie 可用。开发环境分别启动 5173（Vue）与 3000（API），Vite `server.proxy` 指向 API，避免 CORS。

### 8.2 前端架构（Vue 3 SPA）

```
apps/web/src/
├── main.ts / App.vue
├── router/            # 路由与守卫
├── stores/            # pinia: user, map, progress, records
├── api/               # 类型化 fetch 封装（共享 packages/shared 类型）
├── components/
│   ├── map/           # MapCanvas, GlowLayer, RouteLayer, AdminFillLayer, ProgressRing
│   ├── record/        # RecordForm, RecordCard, PhotoGrid, ImportWizard
│   └── common/        # AdminCascader, TransportSelect, EmptyState
├── views/             # 与路由一一对应
├── composables/       # useGeolocation, useReverseGeocode, useProgress
└── styles/            # bootstrap 变量覆盖 + 自定义主题
```

要点：

- 地图组件**不持有业务状态**，只接收 GeoJSON props 并 emit 事件，便于测试与替换底图；
- 行政区级联选择器数据来自 `/api/geo/tree`，前端 IndexedDB 缓存 7 天；
- 所有 API 类型集中在 `packages/shared`，用 Zod 推导，避免前后端字段漂移。

### 8.3 后端与 API

**路由约定**（Next.js `apps/api/app/api/**/route.ts`，统一返回 `{ code, message, data }`）：

| 方法 | 路径 | 说明 | 优先级 |
| --- | --- | --- | --- |
| POST | `/api/auth/login` `/api/auth/logout` | 登录/登出 | P0 |
| GET | `/api/me` | 当前用户与设置 | P0 |
| GET/POST | `/api/visits` | 列表（分页/筛选）/ 新建 | P0 |
| GET/PATCH/DELETE | `/api/visits/[id]` | 详情/编辑/删除 | P0 |
| POST | `/api/import/photos` | 上传+解析（multipart） | P0 |
| GET | `/api/import/jobs/[id]` | 导入任务状态 | P0 |
| POST | `/api/import/jobs/[id]/confirm` | 确认候选批量入账 | P0 |
| GET | `/api/geo/reverse?lat&lng` | 逆地理编码 | P0 |
| GET | `/api/geo/tree?country=` | 行政区树/级联数据 | P0 |
| GET | `/api/progress` | 全局/分国家进度 | P0 |
| GET | `/api/map/features` | 光点+连线 GeoJSON（按筛选） | P0 |
| GET | `/api/stats/overview` | 统计概览 | P1 |
| GET | `/api/export?format=json\|csv` | 数据导出 | P1 |

**鉴权**：HttpOnly + SameSite=Lax Cookie 存会话 ID，Redis 存会话；CSRF 仅对写接口校验 Origin；写接口限流 `rate:{userId}:{route}`（如 60 次/分钟）。

**错误码**：`400` 参数错误（附字段级 message）、`401` 未登录、`403` 越权、`404` 不存在、`409` 重复记录、`413` 文件过大、`422` 业务校验失败、`429` 限流、`500` 服务异常。

### 8.4 数据库选型

| 候选 | 优势 | 劣势 | 结论 |
| --- | --- | --- | --- |
| **PostgreSQL 16 + PostGIS** | 点面判断 `ST_Intersects`、就近归属 `ST_DWithin`、地理聚合、JSONB、物化视图；周边工具成熟 | 需要独立服务与备份 | ✅ **默认方案** |
| SQLite + R-tree | 零运维、单文件、适合纯本地单用户 | 无 PostGIS，边界判定需自研射线法；并发写弱；Docker/多端同步困难 | 作为「纯本地单用户」备选，P2 再评估 |
| MySQL 8 | 生态广 | 地理能力弱（SRID 支持有限）、窗口函数可用但边界数据索引麻烦 | 不采用 |
| MongoDB | 文档模型灵活 | 行政区边界与聚合统计不适合；一致性弱 | 不采用 |

> **按需选择**的落地方式：MVP 明确只支持 PostgreSQL（`DATABASE_URL` 指向自托管或 Supabase/Neon 等托管实例）；若后续确认只做单机单用户，再评估 SQLite 适配（Prisma 需拆分 datasource，成本高，暂不承诺）。

### 8.5 数据模型（Prisma 草案）

```prisma
enum VisitType { STAY TRANSIT }
enum TransportMode { PLANE HSR TRAIN BUS CAR BIKE WALK FERRY OTHER }

model User {
  id        String   @id @default(cuid())
  email     String?  @unique
  password  String?
  name      String?
  settings  Json     @default("{}")
  createdAt DateTime @default(now())
  visits    Visit[]
  trips     Trip[]
  media     Media[]
  places    Place[]
}

// 行政区：level 0=国家 1=一级行政区 2=城市
model AdminDivision {
  id        String  @id @default(cuid())
  level     Int
  code      String  @unique      // ISO 3166-1 / ISO 3166-2 / 国标码
  parentId  String?
  nameZh    String
  nameEn    String
  parent    AdminDivision?  @relation("Tree", fields: [parentId], references: [id])
  children  AdminDivision[] @relation("Tree")
  // geometry / centroid 用 PostGIS 原生列，由迁移 SQL 添加
  @@index([level, parentId])
  @@index([nameZh])
}

model Place {
  id         String  @id @default(cuid())
  userId     String
  name       String?            // 具体地点名
  lat        Float?
  lng        Float?
  countryId  String?
  provinceId String?
  cityId     String?
  user       User    @relation(fields: [userId], references: [id])
  visits     Visit[]
  @@index([userId])
}

model Trip {
  id       String  @id @default(cuid())
  userId   String
  title    String
  startAt  DateTime
  endAt    DateTime?
  note     String?
  user     User    @relation(fields: [userId], references: [id])
  visits   Visit[]
}

model Visit {
  id            String        @id @default(cuid())
  userId        String
  tripId        String?
  placeId       String
  visitType     VisitType    @default(STAY)
  startAt       DateTime
  endAt         DateTime?
  transportMode TransportMode?
  fromPlaceId   String?
  note          String?
  rating        Int?
  createdAt     DateTime      @default(now())
  user          User          @relation(fields: [userId], references: [id])
  trip          Trip?         @relation(fields: [tripId], references: [id])
  place         Place         @relation(fields: [placeId], references: [id])
  photos        Media[]
  @@index([userId, startAt])
  @@index([userId, visitType])
}

model Media {
  id         String   @id @default(cuid())
  userId     String
  visitId    String?
  storageKey String              // 原图或空
  thumbKey   String
  previewKey String
  takenAt    DateTime?
  lat        Float?
  lng        Float?
  hash       String              // SHA-256 去重
  status     String   @default("READY") // PENDING/READY/FAILED
  user       User     @relation(fields: [userId], references: [id])
  visit      Visit?   @relation(fields: [visitId], references: [id])
  @@unique([userId, hash])
  @@index([visitId])
}
```

> PostGIS 列通过原生迁移补充：
> `ALTER TABLE "AdminDivision" ADD COLUMN geom geography(MultiPolygon,4326), ADD COLUMN centroid geography(Point,4326);`
> 并建 `GIST` 索引。

### 8.6 Redis 缓存设计

| Key | 类型 | TTL | 用途 | 失效 |
| --- | --- | --- | --- | --- |
| `sess:{sid}` | string(JSON) | 7d | 会话 | 登出删除 |
| `progress:{userId}:v1` | string(JSON) | 1h | 进度快照 | Visit 增删改即 DEL |
| `geo:rev:{lat4}:{lng4}` | string(JSON) | 30d | 逆地理结果（坐标截 4 位小数约 11m，聚合缓存友好） | 自然过期 |
| `geo:rev:miss:{lat4}:{lng4}` | string | 1d | 负缓存，防穿透 | 自然过期 |
| `geo:tree:{country}` | string(JSON) | 24h | 行政区树 | 数据版本升级时 `admin:ver` 变更 |
| `import:job:{jobId}` | hash | 24h | 导入进度/状态 | 完成后 24h |
| `rate:{userId}:{route}` | counter | 60s | 限流 | 自然过期 |
| `lock:progress:{userId}` | string | 10s | 重算互斥锁 | 释放 |

一致性策略：**Cache-Aside + 写后失效**；进度重算加锁防击穿；Redis 不可用时降级直查 DB（记录告警），不阻塞录入。

### 8.7 关键算法

**1) 照片坐标 → 行政区（避免全量扫边界）**

```
1. 城市级：SELECT id FROM "AdminDivision" WHERE level=2 AND ST_Intersects(geom, pt) LIMIT 1
2. 未命中 → 省级 / 国家级依次判定
3. 仍未命中 → 就近质心：ORDER BY centroid <-> pt LIMIT 1，且 dt <= 50km 才采纳
4. 全部失败 → 返回 null，标记「待手动定位」
5. 结果写入 geo:rev 缓存
```

**2) 解锁进度聚合（单条 SQL）**

```sql
SELECT
  COUNT(DISTINCT p."countryId")  AS countries,
  COUNT(DISTINCT p."provinceId") AS provinces,
  COUNT(DISTINCT p."cityId")     AS cities
FROM "Visit" v JOIN "Place" p ON p.id = v."placeId"
WHERE v."userId" = $1
  AND v."visitType" = 'STAY';   -- 设置开启「途经计入」时改为 IN ('STAY','TRANSIT')
```

分母来自 `AdminDivision` 按 `level` 计数（带 `hasChildrenData` 标记），前端只展示有分母的层级。

**3) 连线 GeoJSON**

- 排序：`ORDER BY startAt`；
- 飞机：`@turf/great-circle` 生成大圆，按经度 ±180 处切段；
- 地面：LineString（P2 可接路线规划 API 但会引入配额与合规风险）；
- 采样：点数 ≤ 200 时全量，> 200 时按 Douglas-Peucker 简化（容忍度随 zoom）。

**4) 照片聚类**

```
按 takenAt 升序；相邻照片若 Δt ≤ 12h 且 距离 ≤ 20km 归入同簇；
簇的主地点 = 簇内经停最久/最中心的城市；置信度 = f(簇内点数, 距离离散度)；
单张照片簇置信度低，需用户确认。
```

### 8.8 行政区划数据源

| 数据 | 来源 | 精度 | 许可注意 |
| --- | --- | --- | --- |
| 国家边界 | Natural Earth 1:50m / ISO 3166-1 | 中 | 公共领域（NE） |
| 一级行政区 | Natural Earth admin-1（全球）/ 民政部（中国） | 中 | GADM 仅限非商业，慎用；优先 NE + 官方 |
| 城市/县级 | GeoNames + 民政部行政区划代码（中国） | 高（中国） | GeoNames CC-BY 4.0 需署名 |
| 中国口径 | 34 省级（含港澳台）+ 333 地级行政区 | 高 | 数据版本化，升级需迁移脚本 |

> 数据以 `data/admin/*.geojson` + 种子脚本入库；记录写入时固化 `adminCode` 快照，避免升级后统计漂移。隐私与合规：底图使用需确认审图号/瓦片授权（见风险 R2）。

### 8.9 部署与运维

**Docker Compose 服务**：`spa`（Nginx 托管 Vue 构建产物 + 反代 `/api`）、`api`（Next.js standalone）、`postgres`（postgis/postgis:16-3.4）、`redis:7-alpine`、`minio`。数据卷持久化；每日 `pg_dump` + MinIO 同步备份。

**Vercel 方案的限制**：

- Route Handler 默认超时短，照片批处理应走 `maxDuration` 上限内的小批量或独立 worker；
- 无状态文件系统，照片必须用 S3/R2；Redis 用 Upstash；Vue SPA 可放任意静态托管（需为 API 配置 CORS 与 Cookie `SameSite=None; Secure`）；
- **结论**：MVP 推荐自托管 Compose；纯托管方案中 Vercel 只承担 API 层，导入走外部队列。

**当前设备环境（2026-11-24 核查）**：git 2.56 可用；**Node.js、pnpm、gh CLI 未安装**，git 全局身份未配置。M1 开工前需安装 Node 20 LTS+、pnpm 9+，并配置 `user.name/user.email` 与 GitHub 凭据（HTTPS PAT 或 SSH key），否则无法完成 `git push`。

### 8.10 目录结构（Monorepo 草案）

```
TravelDiary/
├── apps/
│   ├── web/                  # Vue 3 SPA：Vite + Vue Router + Pinia + Bootstrap 5 + MapLibre
│   ├── api/                  # Next.js：仅 App Router Route Handlers（不写页面）
│   └── worker/               # 可选：照片批处理/进度重算（P1）
├── packages/
│   ├── shared/               # Zod schema、类型、常量（交通方式/口径枚举）
│   └── admin-data/           # 行政区 GeoJSON 与种子脚本
├── docs/
│   ├── PRD.md
│   └── api.md                # OpenAPI/接口说明
├── docker/                   # compose（spa/api/postgres/redis/minio）、nginx、初始化 SQL
├── data/                     # 本地对象存储挂载（gitignore）
├── .env.example
└── package.json              # pnpm workspace
```

### 8.11 关键环境变量

```
DATABASE_URL=postgresql://user:pass@localhost:5432/traveldiary
REDIS_URL=redis://localhost:6379
AUTH_MODE=local            # local | email
SESSION_SECRET=...
MEDIA_DRIVER=local         # local | s3
S3_ENDPOINT / S3_BUCKET / S3_ACCESS_KEY / S3_SECRET_KEY
MAP_TILE_URL=...           # 瓦片模板
GEOCODE_PROVIDER=local     # local | nominatim（仅坐标）
UPLOAD_MAX_MB=20
IMPORT_MAX_FILES=100
```

---

## 9. 非功能需求

| 类别 | 要求 |
| --- | --- |
| 性能 | 首屏 LCP ≤ 2.5s（3G Fast）；地图拖动 60fps；API P95 ≤ 300ms（本地）；进度接口命中缓存 ≤ 50ms；100 张照片导入 ≤ 3min（异步） |
| 容量 | 单用户 10,000 条 Visit、50,000 张照片；单条记录 ≤ 20 张照片 |
| 安全 | 密码 bcrypt(cost≥12)；会话 HttpOnly+Secure；写接口 Origin/CSRF 校验；上传白名单 + 魔数校验 + 大小限制；SQL 全参数化；对象存储路径不可猜测（UUID），禁止路径穿越 |
| 隐私 | 不采集精确定位；无第三方分析脚本默认开启；EXIF 不返回；导出/分享脱敏；提供一键删除 |
| 可用性 | 错误提示含「下一步动作」；草稿自动保存；离线时地图缓存瓦片可浏览、录入暂存本地队列（P1） |
| 兼容性 | Chrome/Edge/Safari 最近 2 个大版本；iOS Safari 16+；最小宽度 375px；深色模式跟随系统 |
| 可访问性 | 对比度 ≥ 4.5:1；键盘可完成录入；进度百分比有 `aria-label`；动效遵循 `prefers-reduced-motion` |
| 可维护性 | 前后端类型共享；API 契约有 Zod；核心算法（口径/聚类/大圆）单测覆盖 ≥ 80%；迁移脚本版本化 |
| 可扩展 | 数据表均带 `user_id`；交通方式与口径枚举集中定义；地图瓦片与存储驱动可配置 |

---

## 10. 数据分析与指标

| 指标 | 定义 | 目标 |
| --- | --- | --- |
| 激活率 | 注册后 7 天内创建 ≥ 3 条记录的用户占比 | ≥ 60%（内测） |
| 补录完成率 | 发起照片导入并完成确认的比例 | ≥ 70% |
| 导入失败率 | 单张解析失败比例 | ≤ 5% |
| 记录中位数耗时 | 新建一条记录从打开到保存 | ≤ 45s |
| 进度页渗透 | 会话中访问地图/进度页的比例 | ≥ 80% |
| 7 日留存 | 首次使用后第 7 天回访 | ≥ 35% |

埋点原则：仅记录事件名与匿名 ID，不记录坐标与备注内容；默认关闭，隐私设置中可开。

---

## 11. 里程碑与排期

| 里程碑 | 交付物 | 时间 | 验收 |
| --- | --- | --- | --- |
| M0 需求定稿 | 本 PRD + 页面线框 | 2026-11-24 ~ 11-26 | 评审通过、口径无歧义 |
| M1 工程骨架 | Monorepo、Docker Compose、Prisma 迁移、登录、手动录入、地图光点 | 2026-11-27 ~ 12-10 | 可创建记录并在图上看到光点 |
| M2 进度与浏览 | 解锁口径实现、Redis 缓存、国家/省/市三级页面、进度环 | 2026-12-11 ~ 12-24 | 三维百分比与手算一致 |
| M3 导入与连线 | 照片导入全链路、逆地理、聚类确认、连线与方式样式、停留/途经 | 2026-12-25 ~ 2027-01-07 | 100 张照片 ≤ 3min 生成候选 |
| M4 打磨上线 | 统计（P1 基础）、导出、PWA、性能与安全自测、部署文档 | 2027-01-08 ~ 01-14 | 走查清单全绿，部署可复现 |

> 排期按 1 名全栈开发估算；若并行增加人力，M2/M3 可压缩约 30%。

---

## 12. 验收标准（Given / When / Then）

1. **手动录入**：Given 已登录用户处于地图页，When 填写「2026-10-01 成都 · 停留 · 飞机」并保存，Then 地图出现成都光点、进度城市数 +1，且 `GET /api/progress` 返回的 `cities` 与其一致。
2. **途经不虚增**：Given 用户首次到访重庆且类型为「途经」，When 保存后查看进度，Then 重庆不计入已解锁城市数；将类型改为「停留」后，进度 +1。
3. **重复记录合并**：Given 同日同城已有 1 条停留记录，When 再建一条同城同日记录，Then 提示合并；选择合并后城市数不变、`visit_count` = 2。
4. **照片导入**：Given 上传 20 张带 GPS 的 JPEG，When 任务完成，Then 生成候选列表并按 12h/20km 聚类，逐条可改行政区；确认后 Visit 与 Media 关联成功，缩略图可显示。
5. **无 GPS 照片**：Given 上传 5 张无 GPS 照片，When 查看候选，Then 全部进入「待定位」分组，可批量指定城市后一次入账。
6. **去重**：Given 已导入照片 A，When 再次导入同一文件，Then 提示「已存在」且不产生重复 Media。
7. **进度口径**：Given 用户到访中国 2 个省级行政区、3 个城市，When 打开进度页，Then 显示「中国：2 / 34 个省级行政区」「3 城」，人工核算百分比一致（保留 1 位小数）。
8. **连线方式**：Given 北京 → 上海两条记录分别标记飞机与高铁，When 查看地图，Then 飞机段为弧线虚线、高铁段为实线，颜色与图例一致。
9. **缓存失效**：Given 进度已缓存，When 新增一条记录，Then 下一次 `GET /api/progress` 返回新数值（缓存已失效）。
10. **越权保护**：Given 用户 A 的会话，When 请求用户 B 的记录 ID，Then 返回 403/404 且不泄露内容。
11. **隐私**：When 调用记录详情接口，Then 响应中不含 EXIF GPS 字段与原图绝对路径。
12. **性能**：Given 2,000 条记录，When 打开地图并拖拽缩放，Then 平均帧率 ≥ 50fps，进度接口 P95 ≤ 300ms。

---

## 13. 风险与待确认问题

### 13.1 风险

| ID | 风险 | 影响 | 缓解 |
| --- | --- | --- | --- |
| R1 | 行政区划数据版权/精度不足，统计口径被质疑 | 高 | 优先 Natural Earth + 官方数据；标注数据版本与来源；提供口径说明页 |
| R2 | 国内底图合规（审图号、商用授权） | 高 | 优先天地图（需 key）或自建离线瓦片；上线前确认授权 |
| R3 | HEIC 解析与浏览器兼容 | 中 | 服务端 heic-convert 转码；失败降级为「仅记录时间/位置」 |
| R4 | 逆地理编码配额与隐私 | 中 | 本地边界优先，在线服务仅传坐标，结果缓存 30 天 |
| R5 | 照片批处理超时（Serverless） | 中 | 自托管 + 异步任务；前端分片上传 |
| R6 | 分母口径争议（195 vs 249） | 中 | 设置项可切换，默认 195，页面注明 |
| R7 | 地图性能随点数增长下降 | 中 | 聚合 + 简化 + 按视口下发 |
| R8 | 当前开发环境缺 Node/pnpm/GitHub 凭据 | 高（阻塞） | M1 前完成环境安装与仓库推送配置 |

### 13.2 待确认问题（Open Questions）

| ID | 问题 | 建议默认值 |
| --- | --- | --- |
| Q1 | 首版是**个人单机工具**还是**多用户在线服务**？ | 单机优先 + 预留多用户（`AUTH_MODE` 切换） |
| Q2 | 部署环境：本地 Docker / 云服务器 / Vercel？ | 本地 Docker Compose |
| Q3 | 地图供应商与授权是否具备（天地图 key / 商用授权）？ | 先用 MapTiler/OSM 免 key 瓦片做开发，上线前确认 |
| Q4 | 港澳台与「国家数」统计口径？ | 港澳台归入中国省级；国家分母 195，可切 249 |
| Q5 | 照片原图是否长期保留？ | 默认保留；设置可改「仅缩略图」与 30 天自动清理 |
| Q6 | 是否需要移动端 App？ | 不做，响应式 + PWA |
| Q7 | 上传到 GitHub 的凭据方式（HTTPS PAT / SSH）？ | 需用户提供，当前环境未配置 |

---

## 14. 附录

### 14.1 术语表

| 术语 | 含义 |
| --- | --- |
| 光点 | 地图上表示一个已解锁城市的发光点 |
| 光路 | 按时间顺序连接光点的连线 |
| 解锁 | 某行政区出现至少一条计入统计的停留记录 |
| 途经 | 中转/路过，默认不计入解锁 |
| 探索度 | 加权后的综合百分比（国家 50% + 省 30% + 城市 20%） |
| 探索值 | 用于等级/成就的绝对分值（城市 10 + 省 30 + 国家 100） |

### 14.2 参考与依赖

- Natural Earth：https://www.naturalearthdata.com/
- GeoNames：https://www.geonames.org/
- ISO 3166：https://www.iso.org/iso-3166-country-codes.html
- MapLibre GL JS：https://maplibre.org/
- PostGIS：https://postgis.net/
- 天地图：https://www.tianditu.gov.cn/
- 航旅纵横（交互参考）：飞行记录 → 航线图 → 城市清单

---

*本文档为 v1.0 草案，评审通过后冻结为 v1.1 基线，需求变更需在修订记录中登记。*
