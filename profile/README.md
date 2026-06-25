# openIndu Community

**开源智能制造解决方案 | Open Source Intelligent Manufacturing**

> 开源 · 开放 · 协作 — 致力于智能制造场景，提供 AI 赋能的工业互联网解决方案

融合运动控制、机器视觉、工业物联网平台与 AI 基础设施，由社区共同构建完整的智能制造生态。

---

## 核心项目 | Projects

### 🌐 社区官网生态

| 仓库 | 技术栈 | 说明 |
| ---- | ------ | ---- |
| [openIndu-website](https://github.com/openIndu/openIndu-website) | Monorepo | 聚合仓 — 统一管理后端、管理台、官网前台 |
| [openIndu-backend](https://github.com/openIndu/openIndu-backend) | FastAPI · PostgreSQL · Milvus | REST API + MCP Server，支持 RAG 知识库检索 |
| [openIndu-admin](https://github.com/openIndu/openIndu-admin) | React 19 · Tailwind CSS 4 | 统一管理后台 — 用户、文档、软件、系统配置 |
| [openIndu-portal](https://github.com/openIndu/openIndu-portal) | React 19 · Tailwind CSS 4 | 社区官网前台 — 知识库、软件下载、方案展示 |

### 🏭 工业平台

| 仓库 | 状态 | 说明 |
| ---- | ---- | ---- |
| [openIndu-platform](https://github.com/openIndu/openIndu-platform) | ✅ 可用 | 企业级工业物联网全栈解决方案 — 设备管理、流程控制、产品追溯一体化 |
| [openIndu-studio](https://github.com/openIndu/openIndu-studio) | ✅ 可用 | AI 工业知识库工作台 — PDF 解析、向量检索、RAG 问答 |
| [openplc-runtime](https://github.com/openIndu/openplc-runtime) | ✅ 可用 | OpenPLC 运行时适配与扩展 |
| [openIndu-controller](https://github.com/openIndu/openIndu-controller) | 🚧 即将推出 | AI + 运动控制 — 结合 AI 算法优化工业设备运动精度与效率 |
| [openIndu-vision](https://github.com/openIndu/openIndu-vision) | 🚧 即将推出 | AI + 视觉 — 基于深度学习的工业视觉检测，实现产品质量自动化检测 |

---

## 服务架构 | Services

### 🌐 社区官网生态 | Community Website

聚合仓 **openIndu-website** 通过 git submodule 统一管理前台、后台与后端三个子仓；用户访问 **openIndu-portal** 官网前台，运营通过 **openIndu-admin** 管理后台，二者共用 **openIndu-backend** 提供的 REST API 与 MCP 知识检索，数据落在 PostgreSQL / Milvus 向量库 / 阿里云 OSS。

```mermaid
flowchart TD
    website["openIndu-website<br/>聚合仓 · Monorepo (git submodule)"]
    website -.聚合.-> portal
    website -.聚合.-> admin

    portal["openIndu-portal<br/>社区官网前台<br/>React 19 + nginx"]
    admin["openIndu-admin<br/>管理后台<br/>React 19 + nginx"]
    backend["openIndu-backend<br/>REST API + MCP Server<br/>FastAPI"]

    portal --> backend
    admin --> backend

    backend --> pg[("PostgreSQL")]
    backend --> milvus[("Milvus<br/>RAG 向量库")]
    backend --> oss[("阿里云 OSS")]

    click website "https://github.com/openIndu/openIndu-website" _blank
    click portal "https://github.com/openIndu/openIndu-portal" _blank
    click admin "https://github.com/openIndu/openIndu-admin" _blank
    click backend "https://github.com/openIndu/openIndu-backend" _blank
```

| 仓库 | 状态 | 职责 |
| ---- | ---- | ---- |
| [openIndu-website](https://github.com/openIndu/openIndu-website) | ✅ 可用 | 聚合仓 — submodule 统一管理前台/后台/后端 |
| [openIndu-portal](https://github.com/openIndu/openIndu-portal) | ✅ 可用 | 社区官网前台 — 知识库、软件下载、方案展示 |
| [openIndu-admin](https://github.com/openIndu/openIndu-admin) | ✅ 可用 | 管理后台 — 用户、文档、软件、系统配置 |
| [openIndu-backend](https://github.com/openIndu/openIndu-backend) | ✅ 可用 | REST API + MCP Server — 数据接口与 AI 知识检索 |

### 🏭 工业平台 | Industrial Platform

**openIndu-studio** 是开发工作流，在开发态把控制程序与配置下发到现场各执行平台；现场有三类并行的执行平台——**openplc-runtime**（软 PLC，逻辑与 IO 控制）、**openIndu-controller**（基于运动控制板卡直接驱动轴运动）、**openIndu-vision**（视觉检测平台，质量自动化判定），三者互不依赖；它们的运行数据统一汇聚到 **openIndu-platform** 工业互联网平台，完成设备管理、流程控制与产品追溯。

```mermaid
flowchart BT
    studio["openIndu-studio<br/>开发工作流<br/>程序开发 · 配置下发"]

    subgraph field["🏭 现场执行层 · Field Execution"]
        direction LR
        plc["openplc-runtime<br/>软 PLC<br/>逻辑 · IO 控制"]
        controller["openIndu-controller 🚧<br/>运动控制平台<br/>板卡直驱轴运动"]
        vision["openIndu-vision 🚧<br/>视觉检测平台<br/>质量自动化判定"]
    end

    iiot["openIndu-platform<br/>工业互联网平台<br/>设备管理 · 流程控制 · 产品追溯"]

    %% 开发态：下发程序/配置（虚线）
    studio -. 程序/配置下发 .-> plc
    studio -. 程序/配置下发 .-> controller
    studio -. 程序/配置下发 .-> vision

    %% 运行数据上抛平台
    plc -- 设备数据 --> iiot
    controller -- 运动数据 --> iiot
    vision -- 质检数据 --> iiot

    click studio "https://github.com/openIndu/openIndu-studio" _blank
    click plc "https://github.com/openIndu/openplc-runtime" _blank
    click controller "https://github.com/openIndu/openIndu-controller" _blank
    click vision "https://github.com/openIndu/openIndu-vision" _blank
    click iiot "https://github.com/openIndu/openIndu-platform" _blank
```

| 定位 | 仓库 | 状态 | 职责 |
| ---- | ---- | ---- | ---- |
| 开发工作流 | [openIndu-studio](https://github.com/openIndu/openIndu-studio) | ✅ 可用 | 开发态工作流 — 程序开发、配置下发到现场各执行平台 |
| 现场执行 · 软 PLC | [openplc-runtime](https://github.com/openIndu/openplc-runtime) | ✅ 可用 | 软 PLC 运行时 — 逻辑与 IO 控制 |
| 现场执行 · 运动控制 | [openIndu-controller](https://github.com/openIndu/openIndu-controller) | 🚧 即将推出 | 运动控制平台 — 基于板卡直接驱动轴运动 |
| 现场执行 · 视觉检测 | [openIndu-vision](https://github.com/openIndu/openIndu-vision) | 🚧 即将推出 | 视觉检测平台 — 工业视觉检测，产品质量自动化判定 |
| 平台 | [openIndu-platform](https://github.com/openIndu/openIndu-platform) | ✅ 可用 | 工业互联网平台 — 汇聚现场数据，设备管理/流程控制/产品追溯 |

---

## 为什么选择开源？ | Why Open Source?

| | |
| --- | --- |
| **完全开源** | 所有核心代码公开透明，支持自由使用、修改和分发 |
| **社区驱动** | 由全球开发者共同参与建设，持续迭代优化 |
| **免费使用** | 无需授权费用，降低企业数字化转型成本 |
| **开放协作** | 欢迎提交 Issue 和 PR，共同打造工业互联网生态 |

---

## 参与贡献 | Contributing

欢迎任何形式的贡献——无论是提交 Issue、发起 PR，还是完善文档。  
请查看 [community](https://github.com/openIndu/community) 仓库了解社区规范与贡献指南。

---

> 📖 **更多详细内容请访问 [www.openindu.com](https://www.openindu.com)**

---

© 2026 openIndu Community
