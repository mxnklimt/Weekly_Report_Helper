# weekly-report

> 一个把零散工作记录变成「可读性高、信号密度大、行动导向」中文周报的 AI Skill。
> 适配 CodeMaker / Claude Skills / Cursor 等支持 skill / system prompt 注入的 AI 平台。

[![version](https://img.shields.io/badge/version-1.2.3-blue.svg)](./SKILL.md)
[![license](https://img.shields.io/badge/license-MIT-green.svg)](./LICENSE)
[![lang](https://img.shields.io/badge/lang-zh--CN-red.svg)]()

---

## 它解决什么问题

写周报这件事，很多人卡在三个地方：

1. **不知道怎么呈现工作量**——明明做了很多事，写出来看着像没干活；或者反过来，注水严重，经理一眼看穿。
2. **不知道写多详细**——给经理看 10 页太长，群里同步两句话又太短。
3. **AI 帮写出来一股「赋能/抓手/闭环/链路」味**——经理读两段就划走。

`weekly-report` 这个 skill 把上面三件事一次性解决：

- **「工作量饱满呈现策略」** 七条机制（拆而不碎、多维度挖掘、量化优先、隐性工作显性化、影响半径外扩、其他工作段、反注水红线）— 让真实的工作量被看见，不靠堆砌。
- **双版本输出**：详细版（六段完整结构）/ 简要版（仅「本周工作 + 下周工作」），按场景切换。
- **反 AI 味黑名单 + 自检清单**：「赋能/抓手/闭环/拉通/对齐颗粒度/大幅提升/显著优化」自动剔除。
- **下周计划只写「做什么」**：忠实于用户工作文档原文，不擅自补交付物、预期收益、复杂度评估。
- **默认自动落盘**：生成后写入 `.md`，文件名规范化（含日期范围 + 版本后缀），方便归档。

---

## 安装

把 `weekly-report/` 整个目录复制到你的 AI 平台 skill 目录即可。

### CodeMaker

```bash
git clone https://github.com/mxnklimt/Weekly_Report_Helper.git \
  ~/.codemaker/skills/weekly-report
```

重启 CodeMaker，在 skill 面板选择 `weekly-report` 即可调用。

### Claude Skills（Claude.ai / Claude Code）

把 `SKILL.md` 内容上传到 Claude 的「自定义 skill」面板，或放到 Claude Code 配置目录：

```bash
git clone https://github.com/mxnklimt/Weekly_Report_Helper.git \
  ~/.claude/skills/weekly-report
```

### Cursor / 其他支持 system prompt 的工具

直接把 `SKILL.md` 第一个 `---` 之后的 markdown 内容粘进 system prompt 或 rules 文件即可使用。

---

## 快速上手

把本周的工作记录贴给 AI，加一句调用指令：

```
调用 weekly-report skill，给我生成一份简要版周报。

本周记录：
- 周一：上线订单导出，运营组已使用 12 次
- 周二-周三：排查支付链路超时，定位到 redis 连接池问题，修复后 P99 1.2s → 0.4s
- 周四：评审同事 PR 5 个，重点关注鉴权改造
- 周五：跨部门对齐 Q3 大促方案，输出会议纪要
- 下周：继续推进鉴权改造 / 大促压测 / 监控告警接入
```

输出（简要版）：

```markdown
# 周报 · 2026-05-21 ~ 2026-05-26

## 本周工作
- 订单导出功能上线：运营组本周已使用 12 次
- 支付链路超时排查与修复：定位 redis 连接池问题，P99 1.2s → 0.4s
- 同事 PR 评审：5 个，重点关注鉴权改造模块
- Q3 大促方案跨部门对齐：输出会议纪要

## 下周工作
1. 推进鉴权改造
2. 大促压测
3. 监控告警接入
```

文件自动保存为 `weekly-report-20260521-20260526-brief.md`。

---

## 两种输出版本

### 详细版

适用：正式周报、跨部门汇报、月度归档、绩效材料。

```markdown
# 周报 · [姓名] · [YYYY-MM-DD ~ YYYY-MM-DD]

## 本周小结
[1-2 句话定位状态]

## 本周完成
- [事项]：[产出/影响/数据]

## 进行中
| 事项 | 进度 | 预计完成 | 备注 |

## 卡点 / 需要支持
- [卡点]：影响 / 需要 / 风险

## 下周计划
1. [按用户文档原文照搬]
```

### 简要版

适用：日报式扎堆汇报、群内同步、每周例会前的速览版、自己回顾用。

```markdown
# 周报 · [姓名] · [YYYY-MM-DD ~ YYYY-MM-DD]

## 本周工作
- [事项概括]：[关键产出/数据]

## 下周工作
1. [按用户文档原文照搬]
```

切换方式：在请求里说「简要版/简版」或「详细版/详版」即可；不指定时 skill 会主动询问。

---

## 设计理念

### 1. 工作量饱满，但反注水

「饱满」 ≠ 「注水」。skill 内置七条饱满策略和五条注水红线：

- **饱满**：颗粒度拆而不碎、多维度挖掘隐藏工作量、量化优先、隐性工作显性化、影响半径外扩……
- **注水（禁止）**：同一件事重复条目、把正常职责包装成成就、凑无意义数字、动作冒充结果、编造数据。

宁可短而真实，不要长而虚。

### 2. 下周计划忠实原文

很多 AI 写周报喜欢自作主张帮你给下周计划补交付物、预期收益、复杂度评估。这版 skill 反其道而行：

> **下周工作只写「做什么」事项本身**——即使用户原文文档里有预期收益/复杂度等列，也一律剥离。

理由：你交给经理的下周计划，应该和你内部规划文档保持一致；AI 不应该替你做承诺。

### 3. 默认落盘

生成完直接写 `.md` 文件，命名 `weekly-report-YYYYMMDD-YYYYMMDD-[detailed|brief].md`，方便归档复盘。

路径优先级：用户指定 > `weekly-reports/` > `reports/` 或 `docs/weekly/` > 当前目录。

---

## 文件结构

```
weekly-report/
├── SKILL.md           # skill 主体（系统提示词）
├── README.md          # 本文件
└── LICENSE            # MIT
```

只有 `SKILL.md` 一个核心文件。所有规则都在里面，可读、可改、可 fork。

---

## 自定义建议

打开 `SKILL.md` 直接改，重点可以调的几个地方：

- **目标受众**（默认「直属经理 / 团队内部」）— 改成高管/客户/自己回顾时，对应模板和语气可以调整。
- **AI 味黑名单**（搜「赋能、抓手、闭环」）— 加上你公司特别讨厌的 buzzword。
- **保存目录优先级**（搜「保存路径优先级」）— 改成你团队约定的路径。
- **命名规范**（搜「文件命名规范」）— 想把日期格式从 `YYYYMMDD` 改成 `YYYY-MM-DD` 或加上姓名也行。

---

## 版本历史

| 版本 | 主要变更 |
|------|----------|
| 1.2.3 | 下周计划只写事项本身，剥离预期收益/复杂度等辅助列 |
| 1.2.2 | 下周计划忠实用户文档原文，禁止 AI 自作主张补字段 |
| 1.2.1 | 移除条数硬性限制，长度由素材决定 |
| 1.2.0 | 默认自动落盘，文件命名规范化 |
| 1.1.0 | 新增简要版输出 |
| 1.0.0 | 初版：核心四段 + 工作量饱满策略 |

---

## 致谢

- 范式参考：[claude-office-skills/skills](https://github.com/claude-office-skills/skills) 的 `weekly-report` 范式与 Anthropic 官方 skill 教程。
- 反 AI 味词表灵感：Wikipedia "Signs of AI writing" / [@blader/humanizer](https://github.com/blader/humanizer)。

---

## License

MIT
