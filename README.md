# Travel Diary · 旅行足迹

以「地图解锁」为核心的个人旅行足迹记录工具。支持**手动录入**与**照片批量导入**两条通道，按「国家 → 省/州 → 城市」三级统计解锁进度与百分比，并在深色地图上以**光点 + 连线**展示足迹光路，每条记录可标注出行方式（飞机 / 高铁 / 自驾 / 徒步…）。

| | |
| --- | --- |
| 蓝图 | [docs/PRD.md](docs/PRD.md) v1.0（待评审） |
| 前端 | Vue 3 + TypeScript + Vite + Bootstrap 5 + MapLibre GL JS |
| 后端 | Next.js Route Handlers（Node.js）+ TypeScript + Prisma |
| 存储 | PostgreSQL 16 + PostGIS（默认）/ Redis 7 / MinIO（照片） |
| 状态 | 需求阶段：PRD 已完成，代码待开工 |

## 为什么做它

- 地图 App 的足迹依赖后台定位，不常驻就漏记；
- 相册地图只能被动导入「拍过照」的地点，中转、路过无处记录；
- 航旅纵横只覆盖民航，高铁、自驾、徒步无法纳入；
- 手账 / Excel 没有地图可视化，也没有可量化的「解锁百分比」。

## 核心能力（规划）

1. **双通道录入**：手填或导入照片（EXIF 定位 + 时间/距离聚类确认）；
2. **三级解锁进度**：国家 / 一级行政区 / 城市，百分比口径公式化、可解释；
3. **光点 + 连线**：按时间串联足迹，连线样式随出行方式变化，飞行走大圆弧；
4. **停留 / 途经**：中转城市默认不计入解锁，避免虚增；
5. **分级浏览**：类似航旅纵横的城市清单，显示每个城市到访次数与首末次时间。

## 文档

- [产品需求文档 PRD](docs/PRD.md) —— 背景、口径、功能需求、技术方案、里程碑、验收标准

## 快速开始（待代码落地）

> 当前仓库尚未包含应用代码；M1 里程碑启动后补全以下步骤。

```bash
# 环境要求：Node.js 20 LTS+、pnpm 9+、Docker
pnpm install
cp .env.example .env
docker compose -f docker/docker-compose.yml up -d   # postgres / redis / minio
pnpm db:migrate && pnpm db:seed                     # 迁移 + 行政区数据
pnpm dev
```

## 许可

待定。
