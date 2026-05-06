# OpenClaw Skills

面向个人agent的Skills集合，用于扩展 AI 助手的能力。

## Skills 列表

### 1. analyze-skill

**触发词：** `/analyze <飞书表格URL>` 或含有「评测/汇总/GSB/得分率/可用率」等关键词

**适用场景：**
- 你从飞书表格拿到了多模型评测数据（如 voice_clone vs voice_id 的 GSB 评测）
- 需要快速生成标准化的中文 Markdown 评测报告
- 需要计算得分率、可用率、GSB 分布等核心指标

**执行操作：**
1. 解析飞书表格 URL，读取评测数据（需包含 winner/GSB 列 + 0/1 指标列 + 1-5 评分列 + Remarks 列）
2. 按维度计算：GSB 分布、得分率、可用率、单项指标拆解
3. 分析 Remarks 文本，提取高频缺陷词和模型归因
4. 生成固定格式的 EU8-hu 样式报告

**返回结果：**
- 标准化的 5 章节 Markdown 报告：
  1. GSB 结果（胜率分布）
  2. 得分率（加权总分）
  3. 可用率（按槽位计）
  4. 单项指标拆解（差值高亮）
  5. 关键洞察（决胜维度 + 选型建议）

**示例：**
```
用户：/analyze https://bytedance.larkoffice.com/sheets/xxx
→ 返回：EU8 hu 语种 voice_clone vs voice_id 测评汇总报告
```

---

### 2. pm-skill

**触发词：** `/pm`、`/pm senior`、`/pm leader`、`/pm config`

**适用场景：**
- **Senior PM 执行视角：** 需求已确定，需要制定执行计划、任务拆解、详细排期
- **PM Leader 宏观视角：** 需求初期，需要评估 ROI、风险、可行性，判断是否值得做
- **完整分析：** 新项目立项，需要同时回答「值不值得做」和「怎么做」

**三种模式：**

| 模式 | 触发方式 | 输出内容 |
|------|---------|---------|
| **Senior PM** | `/pm senior` 或关键词「排期/执行/落地/拆解」 | 任务 WBS 拆解、详细排期（含里程碑）、资源依赖梳理、上线检查清单 |
| **PM Leader** | `/pm leader` 或关键词「ROI/值得/风险/战略/可行性」 | 战略契合度评估、ROI 分析（成本/收益/回收周期）、风险识别与应对、Go/No-Go 建议 |
| **完整模式** | `/pm` 或「完整分析/立项分析」 | 先宏观评估，若建议「做」则输出执行方案 |

**执行操作：**
1. 检测/读取用户 Profile（`~/.pm-skill-profile.json`），首次使用会引导创建
2. 根据触发模式读取对应参考文档（`references/senior-pm.md` 或 `leader-pm.md`）
3. 应用 Profile 偏好（行业、公司规模、关注领域等）调整分析框架
4. 按模板输出结构化分析报告

**返回结果：**
- **Senior PM 模式：** 可直接落地的执行方案（任务粒度 3-5 天、明确交付物、关键依赖）
- **PM Leader 模式：** 决策级评估报告（数据支撑的 ROI、风险项清单、明确建议）
- **完整模式：** 立项级文档（战略评估 + 执行规划 + 综合结论）

**Profile 管理：**
- `/pm config` - 配置/更新你的角色、行业、偏好
- `/pm whoami` - 查看当前配置

**示例：**
```
用户：我们要做一个直播带货功能，帮我拆解一下任务和排期
→ 触发 Senior PM 视角
→ 返回：WBS 任务拆解、甘特图排期、资源依赖清单

用户：想做一个 AI 智能客服，帮我评估一下值不值得做
→ 触发 PM Leader 视角
→ 返回：ROI 测算、风险评估、可行性结论
```

---

## 使用方法

1. 将 skill 目录复制到你的 OpenClaw workspace skills 目录：
   ```bash
   cp -r analyze-skill pm-skill /path/to/your/openclaw/skills/
   ```
2. 重启 OpenClaw 或热加载技能
3. 使用对应的触发词调用

## License

MIT
