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

### 🏭 工业平台 | Industrial Platform

数据从产线 PLC 自下而上流动：现场设备由 **openplc-runtime** 采集，**openIndu-controller**（运动控制）与 **openIndu-vision**（视觉检测）在边缘侧做实时控制与质检，数据上抛到 **openIndu-platform** 工业互联网平台做设备管理、流程控制与产品追溯，再由 **openIndu-studio** 对工业文档与数据做向量检索 / RAG 问答赋能。

```mermaid
flowchart BT
    subgraph edge["🏭 现场 / 边缘层 · Field & Edge"]
        plc["openplc-runtime<br/>OpenPLC 运行时<br/>设备数据采集"]
        controller["openIndu-controller 🚧<br/>AI + 运动控制<br/>实时精度优化"]
        vision["openIndu-vision 🚧<br/>AI + 视觉检测<br/>质量自动化判定"]
    end

    subgraph platform["☁️ 平台层 · Industrial IoT Platform"]
        iiot["openIndu-platform<br/>工业互联网平台<br/>设备管理 · 流程控制 · 产品追溯"]
    end

    subgraph ai["🤖 AI 赋能层 · AI Enablement"]
        studio["openIndu-studio<br/>AI 工业知识库工作台<br/>PDF 解析 · 向量检索 · RAG 问答"]
    end

    plc -- 设备数据上抛 --> iiot
    controller -- 运动状态/控制反馈 --> iiot
    vision -- 检测结果 --> iiot
    iiot -- 工业数据/文档 --> studio
    studio -- 知识/决策赋能 --> iiot

    click plc "https://github.com/openIndu/openplc-runtime" _blank
    click controller "https://github.com/openIndu/openIndu-controller" _blank
    click vision "https://github.com/openIndu/openIndu-vision" _blank
    click iiot "https://github.com/openIndu/openIndu-platform" _blank
    click studio "https://github.com/openIndu/openIndu-studio" _blank
```

| 层级 | 仓库 | 状态 | 职责 |
| ---- | ---- | ---- | ---- |
| 现场 / 边缘 | [openplc-runtime](https://github.com/openIndu/openplc-runtime) | ✅ 可用 | OpenPLC 运行时适配与扩展 — 采集产线设备数据 |
| 现场 / 边缘 | [openIndu-controller](https://github.com/openIndu/openIndu-controller) | 🚧 即将推出 | AI + 运动控制 — 优化设备运动精度与效率 |
| 现场 / 边缘 | [openIndu-vision](https://github.com/openIndu/openIndu-vision) | 🚧 即将推出 | AI + 视觉 — 工业视觉检测，产品质量自动化判定 |
| 平台 | [openIndu-platform](https://github.com/openIndu/openIndu-platform) | ✅ 可用 | 工业互联网平台 — 设备管理、流程控制、产品追溯一体化 |
| AI 赋能 | [openIndu-studio](https://github.com/openIndu/openIndu-studio) | ✅ 可用 | AI 工业知识库工作台 — PDF 解析、向量检索、RAG 问答 |

### 🌐 社区官网生态 | Community Website

聚合仓 **openIndu-website** 通过 git submodule 统一管理前台、后台与后端三个子仓；用户访问 **openIndu-portal** 官网前台，运营通过 **openIndu-admin** 管理后台，二者共用 **openIndu-backend** 提供的 REST API 与 MCP 知识检索，数据落在 PostgreSQL / Milvus 向量库 / 阿里云 OSS。

```mermaid
flowchart TD
    website["openIndu-website<br/>聚合仓 · Monorepo (git submodule)"]
    website -.聚合.-> portal
    website -.聚合.-> admin
    website -.聚合.-> backend

    portal["openIndu-portal<br/>社区官网前台<br/>React 19 + nginx"]
    admin["openIndu-admin<br/>管理后台<br/>React 19 + nginx"]
    backend["openIndu-backend<br/>REST API + MCP Server<br/>FastAPI"]

    portal -- api.openindu.com --> backend
    admin -- api.openindu.com --> backend

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

© 2026 openIndu Community · [www.openindu.com](https://www.openindu.com)
