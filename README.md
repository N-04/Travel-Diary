# Travel Diary · 旅行足迹

> 以「地图解锁」为核心的个人旅行足迹记录工具：手动录入 + 照片导入，按国家 / 省 / 市统计解锁进度，在地图上以光点与连线串起你的旅行光路。

![status](https://img.shields.io/badge/status-需求阶段-blue)
![Vue 3](https://img.shields.io/badge/Vue-3-42b883?logo=vuedotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952b3?logo=bootstrap&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-API-black?logo=nextdotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16%2BPostGIS-4169e1?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7-dc382d?logo=redis&logoColor=white)

> **当前状态（2026-11-24）**：已完成产品需求文档，应用代码尚未开始。README 中的启动命令为 M1 之后的规划目标，当前仓库可阅读的实质内容是 [docs/PRD.md](docs/PRD.md)。

---

## 目录

- [为什么做它](#为什么做它)
- [核心特性](#核心特性)
- [技术栈](#技术栈)
- [系统架构](#系统架构)
- [目录结构](#目录结构)
- [快速开始](#快速开始)
- [环境变量](#环境变量)
- [解锁口径](#解锁口径)
- [路线图](#路线图)
- [文档](#文档)
- [贡献](#贡献)
- [许可](#许可)

## 为什么做它

旅行记录长期存在一个断层：「去过哪里」要么依赖定位常驻，要么依赖照片被动推断，没有一个工具把足迹当作可管理的资产。

| 现有方案 | 问题 |
| --- | --- |
| 地图 App 足迹 / 时间线 | 需要后台定位常驻，不常驻就漏记；耗电、隐私顾虑大；以「轨迹」为单位，没有「解锁了哪些省/市」的视角 |
| 相册地图 | 只能导入拍过照的地点；中转、路过、当天往返无法记录；无法标注交通方式 |
| 航旅纵横等飞行记录 | 只覆盖民航，高铁 / 自驾 / 徒步 / 轮船无法纳入 |
| Notion / Excel / 手账 | 无地图、无进度、维护成本高 |

本项目的答案是：**双通道录入（手动 + 照片）+ 行政区口径的解锁进度 + 出行方式连线**。

## 核心特性

| 能力 | 说明 | 状态 |
| --- | --- | --- |
| ✍️ 手动录入 | 时间、地点、到访类型、交通方式、备注、评分、照片，60 秒内完成一条 | M1 |
| 📷 照片导入 | 解析 EXIF GPS → 逆地理到行政区 → 按 12h / 20km 聚类 → 人工确认批量入账 | M3 |
| 🗺️ 地图可视化 | 光点按到访次数增辉，连线按交通方式着色，飞机走大圆弧，深色底图 | M1–M3 |
| 📊 解锁进度 | 国家 / 省 / 市三级百分比 + 综合探索度 + 探索值 | M2 |
| 🧭 分级浏览 | 国家 → 省/州 → 城市三层清单，类似航旅纵横的城市记录，带次数与首末次时间 | M2 |
| 🚄 出行方式 | 飞机 / 高铁 / 普速 / 长途汽车 / 自驾 / 骑行 / 徒步 / 轮船 / 其他 | M3 |
| 🚏 停留 / 途经 | 中转城市默认不计入解锁，避免虚增 | M3 |
| 🔍 搜索筛选 | 按年份、国家、交通方式、关键词过滤（P1） | P1 |
| 📤 数据导出 | JSON / CSV 导出，数据可带走（P1） | P1 |
| 🏅 成就体系 | 徽章、等级、年度回顾（P2） | P2 |

## 技术栈

| 层 | 选型 |
| --- | --- |
| 前端 | Vue 3 + TypeScript + Vite + Vue Router + Pinia |
| UI | Bootstrap 5.3（深色主题）+ Bootstrap Icons |
| 地图 | MapLibre GL JS（瓦片源可配置：天地图 / MapTiler / OSM） |
| 图表 | ECharts（P1 统计） |
| 后端 | Next.js App Router Route Handlers（Node.js 运行时，**纯 API，不承载页面**） |
| 校验 | Zod（前后端共享 schema） |
| ORM | Prisma（PostGIS 部分用原生 SQL） |
| 数据库 | PostgreSQL 16 + PostGIS 3.4 |
| 缓存 | Redis 7（会话、进度、逆地理、导入任务、限流） |
| 图片 | sharp + exifr + heic-convert |
| 存储 | 本地卷 / MinIO（S3 兼容） |
| 部署 | Docker Compose + Nginx（Vue 静态资源 + `/api` 反代） |

## 系统架构

```mermaid
flowchart LR
    U[浏览器] --> N[Nginx]
    N -->|静态资源| W[Vue 3 SPA]
    N -->|/api 反向代理| A[Next.js API]
    A --> P[(PostgreSQL<br/>+ PostGIS)]
    A --> R[(Redis)]
    A --> M[(MinIO / 本地卷<br/>照片与缩略图)]
    A -.-> Q[异步任务<br/>照片批处理 P1]
```

- Vue SPA 与 API 同源部署，避免 CORS 与 Cookie 跨站问题；
- 开发环境分别启动 `5173`（Vue）与 `3000`（API），由 Vite `server.proxy` 转发 `/api`；
- 进度结果写入 Redis（`progress:{userId}:v1`），任意记录写操作后主动失效。

## 目录结构

```
TravelDiary/
├── apps/
│   ├── web/                  # Vue 3 SPA（Vite + Bootstrap + MapLibre）
│   ├── api/                  # Next.js API（仅 Route Handlers）
│   └── worker/               # 可选：照片批处理 / 进度重算（P1）
├── packages/
│   ├── shared/               # Zod schema、共享类型、常量
│   └── admin-data/           # 行政区 GeoJSON 与种子脚本
├── docs/
│   ├── PRD.md                # 产品需求文档
│   └── api.md                # 接口说明（待补）
├── docker/                   # compose / nginx / 初始化 SQL
├── data/                     # 本地对象存储挂载（gitignore）
├── .env.example
├── .gitignore
└── package.json              # pnpm workspace
```

## 快速开始

> 应用代码尚未落地，以下为 M1 里程碑后的目标流程，供环境预研参考。

**前置条件**

- Node.js ≥ 20 LTS、pnpm ≥ 9
- Docker（PostgreSQL 16 + PostGIS / Redis 7 / MinIO）
- 可用的地图瓦片源（开发期可用免 key 的 OSM / MapTiler）

```bash
git clone git@github.com:N-04/Travel-Diary.git
cd Travel-Diary

pnpm install
cp .env.example .env          # 填写数据库、Redis、会话密钥

docker compose -f docker/docker-compose.yml up -d
pnpm db:migrate               # Prisma 迁移
pnpm db:seed                  # 导入行政区划数据

pnpm dev                      # 同时启动 web(5173) 与 api(3000)
```

导入行政区数据后，首次打开地图应能看到国家轮廓与「0 / 195」的初始进度。

## 环境变量

| 变量 | 说明 | 示例 |
| --- | --- | --- |
| `DATABASE_URL` | PostgreSQL 连接串 | `postgresql://user:pass@localhost:5432/traveldiary` |
| `REDIS_URL` | Redis 连接串 | `redis://localhost:6379` |
| `AUTH_MODE` | 认证模式：本地单用户 / 邮箱登录 | `local` / `email` |
| `SESSION_SECRET` | 会话签名密钥 | 随机 32 字节 |
| `MEDIA_DRIVER` | 照片存储驱动 | `local` / `s3` |
| `S3_ENDPOINT` / `S3_BUCKET` / `S3_ACCESS_KEY` / `S3_SECRET_KEY` | MinIO / S3 配置 | — |
| `MAP_TILE_URL` | 瓦片模板 | `https://.../{z}/{x}/{y}.png` |
| `GEOCODE_PROVIDER` | 逆地理策略 | `local` / `nominatim` |
| `UPLOAD_MAX_MB` | 单张照片上限 | `20` |
| `IMPORT_MAX_FILES` | 单次导入上限 | `100` |

## 解锁口径

三级统计，分母来自版本化的行政区划数据（默认国家分母 195，可切换 249）：

```
国家解锁率 = 已解锁国家数 / N_country
省级解锁率 = 该国已解锁一级行政区数 / N_prov(country)
城市解锁率 = 该省已解锁城市数 / N_city(province)

综合探索度 = 0.5 × 国家解锁率
           + 0.3 × 已访国家的省均解锁率
           + 0.2 × 已访省份的城市均解锁率
```

计数规则：`停留` 计入解锁，`途经` 默认不计入；同一城市多次到访只解锁一次，但累计次数；删除记录后进度实时回退。完整定义与边界处理见 [PRD §6.5](docs/PRD.md#65-解锁进度与百分比核心)。

## 路线图

| 里程碑 | 内容 | 状态 |
| --- | --- | --- |
| M0 | 需求文档与页面线框 | ✅ 完成 |
| M1 | 工程骨架、数据库、登录、手动录入、地图光点 | ⏳ 待开始 |
| M2 | 解锁口径、Redis 缓存、国家/省/市三级页面 | ⏳ 待开始 |
| M3 | 照片导入全链路、逆地理、聚类确认、连线与出行方式 | ⏳ 待开始 |
| M4 | 统计、导出、PWA、部署与验收 | ⏳ 待开始 |

排期详见 [PRD §11](docs/PRD.md#11-里程碑与排期)。

## 文档

- [产品需求文档 PRD](docs/PRD.md)：背景、用户故事、功能需求、解锁口径、技术方案、验收标准
- `docs/api.md`：接口说明（M1 后补充）

## 贡献

1. 从 `main` 切出分支：`feat/xxx`、`fix/xxx`、`docs/xxx`；
2. 提交信息遵循 Conventional Commits（`feat:`、`fix:`、`docs:`、`chore:`）；
3. 提交 PR 前确保类型检查与单元测试通过；
4. 需求变更先在 PRD 修订记录中登记，再改代码。

## 许可

待定。在许可证确定前，默认保留所有权利。

---

<sub>参考与依赖：Natural Earth、GeoNames、MapLibre GL JS、PostGIS、天地图。产品交互参考航旅纵横的飞行记录与城市清单。</sub>
