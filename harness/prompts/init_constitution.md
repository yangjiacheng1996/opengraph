# opengraph 初始宪法（init_constitution）

> **用法**：本文件是 opengraph 项目的**初始宪法草案**，用于 `/speckit.constitution` 命令的输入。
>
> - 宪法正文位于本文件的 `<!-- CONSTITUTION-BODY-START -->` 与 `<!-- CONSTITUTION-BODY-END -->` 之间，
>   **只有这一段是宪法内容**；上面的说明与下面的注意事项都不是。
> - **推荐用法**：执行 `/speckit.constitution`，把下面宪法正文整段作为参数传入；命令会套用官方模板、
>   填写占位符、递增版本号，并把最终结果写入 `.specify/memory/constitution.md`。
> - **备选用法**：直接把宪法正文保存为 `.specify/memory/constitution.md`，随后用 `/speckit.constitution`
>   做一次版本对齐与治理条款补全。
> - **来源**：由 `harness/prompts/why_this_project.md` 的 **Part A 项目章程** + **Part C 流程约束** 合并改写，
>   并套用 Speckit 官方宪法模板（`Core Principles` / 附加章节 / `Governance` / 版本行）的结构。
> - **待办**：宪法中 `TODO(...)` 标记的字段需要在批准日由 `/speckit.constitution` 填实；
>   标 `[待调研]` 的条款依赖 `harness/reports/` 下的调研结论，调研完成后需回填。

---

<!-- CONSTITUTION-BODY-START -->

# opengraph Constitution

## Core Principles

### I. 零 LangSmith 依赖（NON-NEGOTIABLE）

opengraph 的代码、依赖树、配置与网络行为中，**不得存在任何 LangSmith 痕迹**。

- 依赖树中**不得出现** `langsmith` 包（须可由 `pip show` / `pipdeptree` 举证）。
- 代码中**不得读取** `LANGSMITH_API_KEY` / `LANGSMITH_TRACING` / `LANGSMITH_ENDPOINT` / `LANGSMITH_PROJECT` 等环境变量。
- **不得**向 `smith.langchain.com` / `*.langchain.com` 或任何 LangChain 遥测端点发起出站请求。
- **不得**在无 LangSmith 账号、无 API Key、断网环境下出现功能降级。

**Rationale**：LangSmith 的付费墙正是本项目要剥离的对象。「不共存」是本项目与 LangSmith 的**竞争前提**，
若内核仍隐式依赖 LangSmith SDK，则「剥离」无从谈起。

**Requirement**：**MUST NOT** 引入 `langsmith` 依赖、**MUST NOT** 读取 `LANGSMITH_*`、**MUST NOT** 向 `*.langchain.com` 出站。
每个特性的 PR 与 CI 都必须复核本条；违反即为阻断性缺陷，不接受「临时豁免」。

### II. 内核纯净：零 LangChain 依赖

opengraph **只做图运行内核**，核心层**不得 import langchain**。对外只接受**鸭子类型**。

- 消息对象：任何具备 `.tool_calls`（及 `.content` / `.id` / `.tool_call_id`）属性的对象。
- 模型：任何提供 `invoke(input) -> 消息对象` 的可调用对象。
- 工具：任何 `@tool` 风格的可调用对象，具备 `name` / `args` / `id` 语义。
- 路由函数：任何返回节点名或结束标记（`END`）的可调用对象。
- 状态 schema：使用标准 `Annotated` + 归约器（如 `operator.add`）表达，**不得**自带一套并行状态类型体系。

**Rationale**：openchain（LangChain 替代层）需要在 opengraph 之上重建模型抽象、工具、消息与 agent 装配。
若内核绑死 langchain，openchain 就被绑死，两项目分层随之失效。

**Requirement**：**MUST NOT** 在核心层 import langchain / langchain-core / langchain-*；
**MUST** 以鸭子类型承载上层对象，**MUST NOT** 强制转换用户传入的消息对象。

### III. 验收契约优先：`harness/demo/target.py` 原样可跑

`harness/demo/target.py` 是 opengraph 的 **MVP 验收契约**，是**只读基准**，任何特性都不得修改它。

- opengraph 必须提供与 LangGraph Graph API **同名同签名**的接口：
  `opengraph.graph` 导出 `StateGraph` / `START` / `END`；`StateGraph(schema)`、`add_node(name, fn)`、
  `add_edge(src, dst)`、`add_conditional_edges(src, path_fn, path_map)`、`compile()`、
  `compiled.invoke({"messages": [...]})`、`compiled.get_graph(xray=True).draw_mermaid_png()`。
- `invoke()` 必须返回带 `messages` 键的**完整状态**（不是仅新增消息）；
  归约器语义、`state.get(key, default)`、节点返回部分状态字典时的合并规则、
  `messages[-1].tool_calls` 的条件路由判定，均必须与 LangGraph 一致。
- 该脚本自带的 `langchain.*` 导入是**脚本自身**的要求，不构成 opengraph 的依赖要求；
  opengraph 只需**能处理**这些对象（见原则 II）。
- 图可视化若不阻断执行：`get_graph` 的等价导出（mermaid 文本 / PNG 渲染）**MUST** 提供；
  若 opengraph 不自行渲染 PNG，**MUST** 给出明确的替代调用方式与迁移说明。

**Rationale**：这是「能否不依赖 LangChain 与 LangSmith 而独立跑通」这一目标的唯一可判定形式。
验收标准一旦可被随手修改，项目的存在理由就失去锚点。

**Requirement**：**MUST NOT** 修改 `harness/demo/target.py`；**MUST** 以「只换包名，不改脚本」的方式通过验收。
验收契约的**细化**允许，**削弱**不允许。

### IV. 可诊断性是一等功能（NON-NEGOTIABLE）

不牺牲调试体验。Graph 的运行轨迹（trace）、状态快照（state）、中断（interrupt）、重放（time-travel）
**必须可见、可查、可回溯**。

- 图运行时的每一次节点执行都必须产生可观测记录，**MUST** 包含：节点名、输入状态、输出状态、耗时、错误。
- 中断与恢复（interrupt / resume）、状态回溯（time-travel）**MUST** 可用。
- 可观测协议**MUST** 采用 OpenTelemetry 兼容的追踪协议与语义约定（semantic conventions），
  以便复用现成面板，而不是自造私有格式。
- 可视化层**MUST NOT** 阻断执行；可视化不可用时，核心执行**MUST** 继续。

**Rationale**：诊断能力是 LangSmith 的主要卖点，也是本项目决定开源替代的部分。
把它降级为「以后再说」等于抹掉项目一半的价值主张。

**Requirement**：**MUST** 在 v1 交付中提供 trace / state / interrupt / time-travel 四类能力的最小可用实现；
**SHOULD** 额外提供内嵌轻量可视化 UI。[待调研] 具体技术选型与工作量档位以
`harness/reports/Q6-可视化与可诊断性的规模与可行性.md` 的结论为准。

### V. 人工验收是最终验收标准

自动测试通过**不等于**验收通过。每个特性**MUST** 有人工验证记录。

- 人工验证记录**MUST** 落在 `harness/features/<feature>/` 下。
- 记录**MUST** 写明：验证环境、验证步骤、实际输出、结论、遗留问题。
- MVP 验收基线**必须**跑两遍且都通过：
  1. **fake / 伪模型**跑一遍（CI 可自动化，用于回归）；
  2. **真实模型**再跑一遍（人工验收，用于确认端到端真实行为）。
- 上述两次验证**MUST** 在**不设任何 `LANGSMITH_*` 环境变量**的干净环境下进行。

**Rationale**：本项目要替代的是「跑起来对不对」这件事，自动断言无法覆盖语义正确性；
而 LangSmith 式的诊断能力恰恰依赖真实运行轨迹，只测不跑等于自欺。

**Requirement**：**MUST NOT** 将「CI 通过」作为特性验收完成的唯一依据；
**MUST** 在特性目录留下人工验证记录后方可宣布完成。

### VI. 规格驱动开发（Speckit）

本项目全程采用 **OpenCode + Speckit** 编写代码。每个特性走标准流程：

```
proposal → spec → tasks → 实现 → (人工验证) → 验收
```

- 每个特性**MUST** 先有 proposal（`harness/proposals/`）与 spec，再进入 tasks 与实现。
- tasks **MUST** 可逐条勾选、可追溯到 spec 条目。
- 调研产物**MUST** 先落 `harness/reports/`，结论**MUST** 再转入 `harness/proposals/` 变成设计提案，
  **MUST NOT** 用未落盘的结论直接进入实现。
- 文档与代码**MUST** 同批提交；**MUST NOT** 出现「先写代码后补规格」。

**Rationale**：本项目的核心资产是「可审计的契约」（零依赖红线、API 同名对齐）。
若开发过程不可追溯，这些契约会在迭代中悄悄失守。

**Requirement**：**MUST** 走完整流程；**MUST NOT** 在无 spec 的情况下写实现代码。

### VII. 职责边界与内核契约

本仓库**只做 opengraph（图运行内核）**。openchain（LangChain 替代层）是**后续独立项目**，
基于 opengraph 构建，不在本仓库范围内实现。

- opengraph **MUST NOT** 实现 `init_chat_model`、`@tool` 装饰器、消息体系、`create_agent` 等
  LangChain 层能力；只需**能承载**它们（鸭子类型）。
- opengraph **MUST NOT** 追求覆盖 LangSmith 全部企业功能（Fleet、Marketplace、Agent Registry、
  Context Hub 等），**MUST NOT** 做多语言 / 多模型 provider 矩阵。
- opengraph **MUST NOT** 以「与 LangChain 共存」为验收目标。
- 跨仓库接口**MUST** 对齐「内核最小契约」，opengraph 对上（openchain）承诺接口、
  对下（用户 / 脚本）承诺语义。

**Rationale**：范围一旦模糊，opengraph 会被拖回「再造一个 LangChain」的状态，
两项目分层与 openchain 的独立性同时失效。

**Requirement**：**MUST** 在跨仓库协作前冻结内核最小契约；
[待调研] 契约条目以 `harness/reports/Q8-图运行时最小契约.md` 的清单为准。

### VIII. 证据与术语纪律

结论**必须**可追溯，术语**必须**准确。

- 事实性结论**MUST** 附来源标记（URL 或 `本地路径:行号`）。
- 价格、额度、套餐 **MUST** 标注 as-of 日期（`截至 YYYY-MM-DD`）并附「以官方定价页为准」。
- 无法核实的信息 **MUST** 标 `[未验证]` 并写明缺哪个页面 / 缺哪个字段 / 试过哪些 URL；
  **MUST NOT** 用推测填充表格或结论。
- 发起者的自造表述（「三大基础功能」「六大概念」「tool 循环智能体」等）
  **MUST** 标注为自造表述，**MUST** 先给出官方术语对照表再使用，**MUST NOT** 将自造词当作官方术语。

**Rationale**：本项目的立项依据是一份调研报告；报告若掺入推测，后续所有设计决策都会建在流沙上。
自造术语一旦流入代码与文档，会污染整个生态的沟通。

**Requirement**：**MUST** 遵守引用与 `[未验证]` 标注规范；**MUST NOT** 输出无来源的事实性断言。

## 项目定位与范围（Additional Context）

### 目标

在**复刻 LangGraph 三大基础能力**（状态模型 / 节点 / 边与路由）的前提下，提供**彻底剥离 LangSmith、
且内核不依赖 LangChain** 的图运行与可观测能力。两条参考验收目标：

1. LangGraph Quickstart 示例中仅把 `langgraph` 字样替换为 `opengraph`，即可跑起功能一致的
   tool-calling 循环智能体（已具体化为原则 III）。
2. opengraph 提供不亚于 LangSmith 的可视化与可诊断性（见原则 IV）。

> 两条均为**愿望而非既定事实**，其可行性与工作量由调研 Q3 / Q6 / Q7 / Q8 裁定，
> 结论**只能细化**验收契约，**不能削弱**它。

### 两项目路线图

| 项目 | 定位 | 依赖关系 |
|---|---|---|
| **opengraph**（本仓库） | 图运行内核：状态模型 / 节点 / 边与路由 / 运行时 / 可观测 | 零 LangChain、零 LangSmith 依赖 |
| **openchain**（后续项目） | 替代 LangChain 的上层：模型抽象、工具、消息、agent 构造 | 基于 opengraph 构建 |

### 非目标

- 不在 opengraph 内重建 LangChain 层。
- 不覆盖 LangSmith 全部企业功能。
- 不做多语言 / 多模型完整 provider 矩阵。
- 不追求与 LangChain 共存。

### 术语约定

「三大基础功能」「六大概念」「tool 循环智能体」均为**发起者自造表述，非官方术语**。
官方术语对照表以 `harness/reports/Q4-LangSmith对LangChain的实际影响程度.md` 为准。

## 质量门禁与交付流程（Development Workflow & Quality Gates）

### 目录约定

| 路径 | 用途 |
|---|---|
| `harness/prompts/` | 提示词（调研任务书、宪法等） |
| `harness/reports/` | 调研报告（每题一份 + 汇总报告） |
| `harness/proposals/` | 设计提案（调研结论转化的规格来源） |
| `harness/features/<feature>/` | 每个特性的人工验证记录 |
| `harness/demo/target.py` | MVP 验收契约（**只读**） |
| `harness/docs/` | 同步下来的官方文档快照（**只读**） |

### 依赖白名单 / 黑名单

每个特性**MUST** 复核依赖清单 [待调研]（白 / 黑名单以
`harness/reports/Q3-LangGraph的开源边界.md` 的结论为准）。

- **包层黑名单**：`langsmith` 及其任何传递依赖（原则 I）。
- **配置层黑名单**：读取 `LANGSMITH_*` 等环境变量（原则 I）。
- **网络层黑名单**：向 `*.langchain.com` 的出站请求（原则 I）。
- **核心层黑名单**：import `langchain` / `langchain-core`（原则 II）。

### 质量门禁

每个特性在合入前**MUST** 全部通过：

1. **规格门禁**：有 proposal 与 spec，tasks 可逐条勾选（原则 VI）。
2. **依赖门禁**：依赖树无 `langsmith`，无 `LANGSMITH_*` 读取，无 `*.langchain.com` 出站（原则 I / II）。
3. **契约门禁**：`harness/demo/target.py` 原样可跑（原则 III）。
4. **自动门禁**：CI 全绿，且包含 fake 模型跑通验收基线的一轮（原则 V）。
5. **人工门禁**：`harness/features/<feature>/` 下有完整人工验证记录，含真实模型一轮（原则 V）。
6. **证据门禁**：新增结论均有来源标记，术语准确（原则 VIII）。

## Governance

- 本宪法**优先于**其他一切实践与文档约定；与本文件冲突的既有做法，以本文件为准。
- **修订流程**：修订提案 **MUST** 说明「变更了什么 / 为什么 / 受影响的原则与特性 / 迁移方案」；
  原则增删或语义重定义**MUST** 经发起者批准，并**MUST** 附带对已实现特性的影响评估与迁移计划。
- **版本策略**：遵循语义化版本。
  - **MAJOR**：原则被移除或语义重定义（向后不兼容的治理变更）。
  - **MINOR**：新增原则 / 章节，或对既有原则的实质性扩充。
  - **PATCH**：措辞澄清、错别字、非语义性修订。
- **合规复核**：每次修订**MUST** 复核原则 I–VIII 的可验证性；
  尤其**MUST** 检查新增条款是否可被自动化检查或人工检查明确判定。
- **待定项处理**：`TODO(...)` 字段**MUST** 在批准时由 `/speckit.constitution` 填实；
  `[待调研]` 条款**MUST** 在 `harness/reports/` 出具后回填，**MUST NOT** 在依据缺失时长期悬空。
- **演进触发**：调研结论（Q1–Q8）**MAY** 触发宪法修订，尤其是原则 IV 的可观测技术选型
  与原则 VII 的内核最小契约。

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE) | **Last Amended**: TODO(LAST_AMENDED_DATE)

<!-- CONSTITUTION-BODY-END -->

---

## 附：与调研产物的对应关系

| 宪法条款 | 依赖的调研结论 | 状态 |
|---|---|---|
| I. 零 LangSmith 依赖 | `Q3-LangGraph的开源边界.md`（c 付费墙触发点 / 三层剥离定义）、`Q2-收费结构与付费墙地图.md` | 已有原则，依据待补强 |
| II. 内核纯净 | `Q8-图运行时最小契约.md`（最小契约清单） | 已有原则，契约条目待补 |
| III. 验收契约优先 | `Q3-LangGraph的开源边界.md`（d target.py 逐项裁定） | 已有原则，兼容性清单待补 |
| IV. 可诊断性 | `Q6-可视化与可诊断性的规模与可行性.md`（路线 A / B 选型） | `[待调研]` |
| V. 人工验收 | 无（流程性条款） | 已定 |
| VI. 规格驱动 | 无（流程性条款） | 已定 |
| VII. 职责边界 | `Q8-图运行时最小契约.md`（第 4 问）、`Q7-开源边界地图与许可证合规.md` | 部分 `[待调研]` |
| VIII. 证据与术语 | `Q4-LangSmith对LangChain的实际影响程度.md`（官方术语对照表） | 已定 |
| 依赖白 / 黑名单 | `Q3-LangGraph的开源边界.md`（c） | `[待调研]` |
