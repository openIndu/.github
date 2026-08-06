# openIndu

**一栈贯通，开放智造**

*工业自动化的端到端开源操作系统*

> 从工艺约束到产线数据闭环，一个栈，零厂商锁定。

**[openindu.com](https://www.openindu.com)** · [English](README.md) · Apache-2.0 · 开源 · 开放 · 协作

---

## 不是又一个工控工具。是工控行业一直缺少的操作系统。

工控行业不缺工具，缺的是把工具连起来的东西。

- **工艺知识**——工艺窗口在老师傅脑子里。人走了，参数为什么这么设就没人说得清。
- **工程生成**——电气图、BOM、IO 表、PLC 程序活在四个互不相通的工具里。改一处，四处手工同步。
- **跨品牌执行**——换一个 PLC 品牌，程序全部重写。你的软件被硬件厂商的生态绑死。
- **采集与数据**——运行数据锁在控制器里。要拿出来，得再买一套系统。
- **分析洞察**——良率掉了查不到根因。就算查到了，结论也回不到下一次设计里。

每一段都有人做工具，**没有一段连着下一段**。操作系统做的就是这件事——统一的抽象、共享的驱动模型、一致的接口，让分立的部件变成一个系统。

**openIndu 做的是工控行业的 Linux。**

| Linux 做了什么 | openIndu 做什么 |
|---------------|----------------|
| 开源——全部源码任何人可得、可改、可审计 | **全链路 Apache-2.0 开源**——电气 → BOM → IO → PLC/HMI，每一步都看得见、验得了 |
| 硬件抽象层——应用不关心底层是 Intel 还是 AMD | **跨品牌中立**——同一套设计落到西门子 / 三菱 / 欧姆龙 / 基恩士 / 汇川 |
| 驱动模型——一个驱动，所有应用共享 | **开放品牌映射层**——一位工程师贡献一条映射，整个社区复用 |
| 文件系统——数据以结构化方式存储、检索、共享 | **工艺约束库**——工艺窗口、缺陷判据、节拍模型：结构化、可检索、可校验 |
| 发行版 + 包管理——开箱即用的完整系统 | **行业模板库**——面板 / 汽车 / 锂电工位模板：装上就能改 |

**一个栈，从工艺知识到生产洞察。**

---

## 架构

```mermaid
flowchart LR
    A[工艺知识] --> B[工程生成]
    B --> C[跨品牌执行]
    C --> D[采集与数据]
    D --> E[分析洞察]
    E -.->|修正约束| A
```

| 节点 | 职责 | 归属 |
|------|------|------|
| **工艺知识** | 工艺窗口 · 缺陷图谱 · 节拍模型 | [studio](https://github.com/openIndu/openIndu-studio) |
| **工程生成** | 电气 → BOM → IO → PLC/HMI · 跨品牌生成 | [studio](https://github.com/openIndu/openIndu-studio) |
| **跨品牌执行** | 西门子 / 三菱 / 欧姆龙 / 基恩士 / 汇川——一套设计，任意品牌落地 | [studio](https://github.com/openIndu/openIndu-studio) 产出 |
| **采集与数据** | 协议层用 [Apache PLC4X](https://plc4x.apache.org/)——S7 · Modbus · EtherNet/IP · ADS · OPC-UA——数据落时序库 | [platform](https://github.com/openIndu/openIndu-platform) |
| **分析洞察** | BI · OEE · 良率归因 | [admin](https://github.com/openIndu/openIndu-admin) |

**闭环**：工艺约束限定生成什么 → 程序落到产线上跑，不挑品牌 → 数据回流 → 分析修正约束 → 下一次设计从更好的基线开始。

每一层都有自己的贡献者画像。工艺工程师贡献工艺层，电气工程师贡献模板与品牌映射，数据工程师贡献管道与看板。**不会写 Python 也能贡献**——一条品牌映射、一张 IO 表、一个工艺窗口，都是实打实的贡献。

---

## 核心项目

### [openIndu-studio](https://github.com/openIndu/openIndu-studio) — 工程层 + 工艺层

[![Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue)](https://github.com/openIndu/openIndu-studio/blob/main/LICENSE)

**AI 辅助全链路工控开发工具链。** 电气模组 → 电路图 → BOM → IO 地址表 → PLC 程序 → HMI 画面——六步一个工具，多品牌输出（西门子 / 三菱 / 欧姆龙 / 基恩士 / 汇川）。工艺约束库是生成的护栏：只在工艺窗口内生成。每份产出都带解释和 diff——验不了的代码，就是不敢用的代码。

### [openIndu-platform](https://github.com/openIndu/openIndu-platform) — 互联层 + 数据层

[![Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue)](https://github.com/openIndu/openIndu-platform/blob/main/LICENSE)

**工业物联网平台。** 设备接入、数据采集、产线监控、产品追溯——非标自动化产线的即插即用数据层。协议层不重复造轮子，直接站在 **[Apache PLC4X](https://plc4x.apache.org/)** 上：S7、Modbus、EtherNet/IP、ADS、OPC-UA 等一套 Apache-2.0 驱动栈全覆盖。采集到的数据进入时序库，成为分析与工艺优化的原料。

### [openindu-station](https://github.com/openIndu/openindu-station) — 工站软件

[![Apache-2.0](https://img.shields.io/badge/license-Apache--2.0-blue)](https://github.com/openIndu/openindu-station/blob/main/LICENSE)

**工控站控应用 (C#)。** 运动控制 · 机器视觉 · 扫码读码 · 点胶 · 激光——非标自动化工站的软件基座。硬件买得到从来不是难点，调得通才是。

---

## 参与贡献

开源、开放标准、开放协作。

欢迎任何形式的贡献——Issue、PR、文档，尤其是**模板、品牌映射、IO 表、工艺参数、缺陷图谱**。最小的有效贡献是一条数据条目，不是一个写满代码的 PR。

[community](https://github.com/openIndu/community) 仓库 → 社区规范 · 贡献指南 · 治理体系

---

© 2026 openIndu Community · Apache-2.0 · [www.openindu.com](https://www.openindu.com)
