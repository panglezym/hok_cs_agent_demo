# 王者荣耀 · 多 Agent 智能客服 · 交互演示 Demo

> 一个 AI 产品经理的设计练习 · 完整可交互 · 单文件 HTML · 无需任何依赖


## 🎯 这是什么

这是一个**面向游戏行业的智能客服 Agent 产品设计**的交互式演示 Demo。

假设场景：为王者荣耀（及其海外版 Arena of Valor）设计一套能处理**日均 5 万咨询、7 种语言、21 类意图**的多 Agent 客服系统，Demo 展示了它是怎么工作的。

**不是**：真实可用的生产系统，也不涉及任何内部数据。

**是**：一个产品经理视角的完整设计方案的可视化呈现。

## 🚀 在线体验

**👉 [点击在线预览](https://panglezym.github.io/hok_cs_agent_demo/)**

> 部署到 GitHub Pages 后，该链接会自动跳转到 Demo（仓库里的 `index.html` 已配好重定向）

或者本地打开：下载 `index.html` → 双击用浏览器打开（推荐 Safari / Chrome）。

## ✨ Demo 包含什么

### 两个模式切换
- **对话演示**：用户提问 → Agent 处理过程可视化 → 最终回复
- **多 Agent 架构全景**：三层架构总览（Orchestrator + 5 专域 Agent + 语言适配层）

### 四个精心设计的场景

| 场景 | 类型 | 展示重点 |
|------|------|---------|
| 📌 掉线判负补偿 | 单域 Agent | 基础处理链路：NLU → 子图检索 → Policy 核验 → 回复生成 |
| 🔥 复合问题并行 | 多 Agent 并行 | **核心亮点** · 两个 Agent 同时处理复合问题，时延降低 48% |
| ⚡ Agent 超时升人工 | 降级韧性 | 熔断 → Redis 缓存兜底 → 主动升人工（三级降级） |
| 🌏 泰国玩家退款 | 多语言适配 | 同样问题，国服 7 天过期不能退，东南亚 14 天仍可退 |

### 知识图谱可视化
右侧面板可切换「处理步骤」和「知识图谱检索」视图。**14 个节点径向布局**，查询路径动态高亮金色流光，展示 Agent 每次的多跳推理过程。

### 设计决策卡片
每个场景完成后会弹出**设计决策卡**，解释"为什么这样设计、权衡了什么、放弃了什么"——这是产品经理思考深度的体现，不是单纯的功能演示。

## 🎬 演示截图

### 核心场景：多 Agent 并行处理复合问题


两个专域 Agent 同时跑，Orchestrator 合并结果后生成一条连贯回复。时延从 2.3s 降到 1.2s。

### 知识图谱多跳查询


4 跳查询路径：`Player → Match → Compensation → Order → Skin`

### 多语言 Policy 注入（同一问题，答案相反）


全局 Policy 定义通用框架，东南亚适配器只存 `refund_days: 14` 的 override——**规则继承，不是翻译**。

### 多 Agent 架构全景


## ⌨️ 操作方式

| 快捷键 | 作用 |
|--------|------|
| `→` 或 `空格` | 下一步 |
| `←` | 上一步 |
| `R` | 重置当前场景 |

也可以点击界面上的「下一步 / 上一步 / 自动播放 / 重置」按钮。

## 🏗️ 核心设计思路

### 为什么是多 Agent 架构？

单 Agent 在三个地方遇到天花板：

1. **复合问题的综合时延爆炸** — 串行处理，2 个独立问题时延会叠加
2. **专业深度和知识覆盖的矛盾** — System Prompt 膨胀到 8000+ Token，规则相互干扰
3. **多语言规则膨胀失控** — 7 种语言 × 差异化规则组合爆炸

### 三层架构的解法

```
L1 · 编排层（Orchestration Layer）
├── Orchestrator Agent（NLU + 路由 + 合并）
└── Context Manager（跨 Agent 会话状态）
        ↓ 任务派发（A2A Protocol）
L2 · 专域执行层（Domain Agents · 并行）
├── 账号安全 Agent
├── 支付退款 Agent
├── 对局补偿 Agent
├── 游戏内容 Agent
└── 规则政策 Agent
        ↓ 规则注入（按语言区版本）
L3 · 语言适配层（Language Adapters）
├── 中文适配器（国服规则）
├── 繁中适配器（港澳台）
├── 东南亚适配器（PDPA + 本地支付）
└── 英语适配器（EN-US）
```

### 关键数据

- **5 个**专域 Agent，每个持有独立知识子图 + 微调语料
- **14 节点**知识图谱（Neo4j），分 5 类：核心实体 / 游戏内容 / 行为记录 / 工单处理 / 规则约束
- **时延降低 48%**（复合问题场景，串行 2.3s → 并行 1.2s）
- **目标自动解决率 92%**（从单 Agent 基线 87% 提升）

## 🛠️ 技术实现

- **纯 HTML + CSS + JavaScript**，零外部依赖
- **单文件部署**，下载即可用
- **所有 Agent 处理逻辑都是预设脚本模拟**（不接真实 LLM API）—— 为了演示的稳定性和可复现性
- **兼容现代浏览器**：Safari 14+、Chrome 90+、Edge 90+、Firefox 88+
- **响应式布局**：桌面端 / 移动端都能正常显示

## 📦 文件结构

```
hok-cs-agent-demo/
├── hok_cs_agent_demo.html   # Demo 主文件（单文件，约 2000 行，62KB）
├── index.html                # GitHub Pages 入口，自动跳转到 Demo
├── assets/                   # README 配图
│   ├── cover_main.png
│   ├── 02_architecture.png
│   ├── 04_parallel_execution.png
│   ├── 06_knowledge_graph.png
│   └── 08_multilang_policy.png
├── LICENSE
└── README.md
```

## 🔗 部署到 GitHub Pages

只需 3 步：

1. Fork 或 clone 本仓库到你自己的账户
2. 打开仓库 `Settings` → `Pages`
3. `Source` 选 `Deploy from a branch`，`Branch` 选 `main` + `/ (root)`，保存

大约 1–2 分钟后，访问 `https://你的用户名.github.io/仓库名/` 即可——`index.html` 会自动跳转到 Demo。

## 💭 设计决策思考

关于这个 Demo 背后的完整产品思考——为什么多 Agent 不单 Agent、为什么置信度阈值 0.80 不是 0.85、为什么语言适配器要单独分层——都在我的公众号文章里详细写了：

**📖 [《为王者荣耀设计了一套多 Agent 智能客服》]** · @AI 产品日志

## 📝 License

MIT License · 可自由使用、修改、引用，如果对你有帮助给个 Star ⭐ 就好。

## ✉️ 反馈

特别欢迎**不同意见**——被反驳的次数越多，设计稿的质量就越高。

- 提 Issue：直接在这个 repo 开 issue
- 公众号：@AI 产品日志

---

<p align="center">
  <sub>Built with ☕ and product thinking · 2026</sub>
</p>
