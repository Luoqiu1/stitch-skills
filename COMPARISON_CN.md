# Stitch (Google) vs Claude Design (Anthropic) 对比调研

> 调研时间：2026-04-19
> 调研者：@Luoqiu1
> 相关文件：[RESEARCH_CN.md](./RESEARCH_CN.md)（Stitch Skills 深度调研）

## 背景

- **Stitch / stitch-skills**（Google Labs，本仓库 upstream）：Stitch 在 2025 Google I/O 发布；stitch-skills 仓库 2026-01-16 创建。
- **Claude Design**（Anthropic Labs）：2026-04-17 发布，目前处于 Research Preview 阶段。

本文件对比两家 AI 实验室在"AI 驱动设计生成"这条赛道上的产品形态、目标用户、技术路径差异。

## 一句话定位

- **Stitch / stitch-skills**：**开发者工具链**。Stitch 生成 UI，stitch-skills 把它嫁接到 Claude Code / Cursor / Gemini CLI / Antigravity，**一切在你的 git 仓库里发生**。
- **Claude Design**：**云端产品界面**。在 claude.ai 网页里对话式产出设计稿 / 原型 / PPT / 海报，**一切在浏览器里发生**，可一键 "Open in Claude Code" 交棒给本地编码 agent。

## 相同点

1. 都是 2026 年 AI 实验室级产品（Google Labs vs Anthropic Labs）
2. 都把"设计 → 代码"链路作为核心场景
3. 都承认需要 **design system 约束** 才能不出 "AI slop"
4. 都把 **Claude Code 视为最终代码落地点**（Claude Design 明确导出到 CC；stitch-skills 本身就是 CC 的 skill）
5. 产物都能导出 HTML

## 关键差异

| 维度 | Stitch + stitch-skills | Claude Design |
|---|---|---|
| **产品形态** | 后端 API（Stitch）+ 开源 Skill 库 | 托管 Web 产品 |
| **使用入口** | 终端 / IDE（通过 MCP） | claude.ai/design |
| **发布时间** | Stitch 2025 I/O；skills 2026-01-16 | 2026-04-17（刚发布） |
| **付费门槛** | Stitch 免费；skills Apache 2.0 | Pro / Max / Team / Enterprise 订阅 |
| **底层模型** | Gemini 2.5 Pro / Gemini 3 | Claude Opus 4.7 |
| **开放程度** | Skill 可任意复用 / 扩展（`taste-design` 就是社区贡献） | 闭源，Anthropic 控制 feature 节奏 |
| **产出范围** | 只做 UI 屏幕（Web / Mobile） | UI + 原型 + **PPT** + **海报** ← 范围更广 |
| **设计系统载体** | `.stitch/DESIGN.md` 可版本控制的文件 | "读取代码库与设计文件"自动套用，细节未公开 |
| **到代码的路径** | **端到端在本地**：Stitch HTML → `react-components` skill → AST 校验 → 提交仓库 | **跨端握手**：设计稿在 claude.ai，点 "Open in Claude Code" 交给本地 agent |
| **协作链路** | GitHub / Git 原生（skill 就是文件） | 导出 Canva 给非工程师协作 |
| **反 AI-slop 机制** | `taste-design` 显式禁用 Inter / 纯黑 / 紫色霓虹 / 3 等宽卡片 / 虚假 metrics | 官方未见同等约束框架 |
| **Agent 兼容性** | 跨 agent（Claude Code / Cursor / Gemini CLI / Antigravity） | 只绑 Claude 生态 |
| **多页网站自治** | `stitch-loop` 的 baton 模式可无人值守生成 | 未看到自治多页能力 |
| **国内访问** | Stitch 后端需代理；skill 本体可离线用（如 taste-design 规则） | 整体需代理 + 国际订阅 |

## 战略读解

两边**正在从两个方向夹击同一块地**：

- **Google 路线 — 工程师进路**：「你用 IDE 和 agent 本来就在干活，那我把设计生成也做成 skill 塞进你的 agent。」强调可编程、可定制、可开源。`taste-design` 甚至把"品味"变成规则文件。
- **Anthropic 路线 — 非工程师也能用**：「在网页里和 Claude 聊，直接出 PPT / 海报 / 原型，工程师这步才需要 Claude Code。」强调零门槛、结果即用、产出物多样（连海报都做）。
- **交汇点**：**两家都在承认 Claude Code 是 agent 时代的代码端点**。Google 选择把 skill 直接放进 Claude Code 生态；Anthropic 自家产品也把 Claude Code 作为出口。这对 CC 的地位是双重背书。

## 选型指南

| 你的诉求 | 推荐 |
|---|---|
| 用自己的 IDE + Claude Code，端到端在 git 仓库里做前端 | **stitch-skills**（`stitch-design` + `react-components`） |
| 只要生成 PPT / 海报 / 营销素材 | **Claude Design**（Stitch 完全不碰这块） |
| 想无人值守造一个多页 side project | **stitch-skills** 的 `stitch-loop` |
| 和非工程师设计师协作、导出 Canva | **Claude Design** |
| 想把"设计品味"编码成规则复用到未来所有项目 | **stitch-skills 的 `taste-design`**（可脱离 Stitch MCP 单独用） |
| 企业订阅 + 已有代码库想套用设计系统 | **Claude Design**（宣称自动识别代码库 design system） |
| 国内网络环境 + 低付费预算 | **stitch-skills** 的 taste-design 规则抽出本地用；Stitch MCP 需代理 |

## 一句话总结

**stitch-skills 是把"设计生成"嵌进工程师已有工作流；Claude Design 是给"设计生成"造一个独立的云端工作流。**

前者的护城河是 **开放可扩展 + agent 原生**，后者的护城河是 **产出物广度 + 非工程师可用**。

真要做正经前端项目，当前 stitch-skills 的工程严谨度（AST 校验、taste-design 规则、baton 循环）实际上领先于 Claude Design 的公开能力——但 Claude Design 刚发布不久，值得持续观察。

## 信息来源

- Anthropic 官方公告：<https://www.anthropic.com/news/claude-design-anthropic-labs>
- Claude 帮助中心 · Get started with Claude Design：<https://support.claude.com/en/articles/14604416-get-started-with-claude-design>
- Stitch Skills 仓库：<https://github.com/google-labs-code/stitch-skills>
- Google Labs Stitch：<https://labs.google.com/stitch>
