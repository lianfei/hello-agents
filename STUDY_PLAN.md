# Hello-Agents 学习执行方案

> **版本复核：2026-10-26**
> - 原始课程基线：`45dd84e626a91997294ac8d4d44f18b29a411c6e`（2026-08-18）；学习计划于 2026-08-19 的 `4b0ba3b` 加入，2026-08-20 的 `0a90e80` 更新进度。
> - 本次同步：`f908ac525088c871828d6c0ac11f279e73941190`（2026-10-26），合入上游 `36f583c233ea5cee4537ef93455702b55053af18`（2026-10-14）；复核当天 `git ls-remote` 返回的上游 main 也是该提交。
> - 保留已完成的第 1 周进度；从阶段二继续。课程主线仍然成立，本次重点修正可复现性并补充新增资源。

> **总周期**：12 周核心路线，每天 1.5~2 小时；完整 BFCL/GAIA、Agentic-RL、1.x 迁移另排时间，不计入核心路线。
> **前置条件**：
> - 教程主线 Python 3.10+、pip/conda 可用；章节和综合项目使用独立虚拟环境。HelloAgents 1.0.1 单独使用 Python 3.12/3.13。
> - 一个 OpenAI 兼容 API Key（DeepSeek / Qwen / SiliconFlow 均可，环境配置见 [Extra-Chapter/Extra07-环境配置.md](Extra-Chapter/Extra07-环境配置.md)）
> - Git 基本操作
> **使用方法**：完成一项就把 `[ ]` 改为 `[x]`，每个检查点必须达到验收标准再进入下一阶段。

## 🏷️ 标注体系说明

每条任务按 `【优先级｜类型｜难度】` 标注：

| 维度 | 含义 |
|---|---|
| **优先级** | 🔴 **P0** 核心必做（跳过则后续无法进行）；🟡 **P1** 重要（影响能力完整度，时间紧可压缩但不建议跳过）；🟢 **P2** 可选（按兴趣/方向选学） |
| **类型** | `重点` = 核心技术/高频考点，必须吃透；`难点` = 易卡壳、需多花时间；无标注 = 常规任务 |
| **难度** | ★☆☆ 入门 / ★★☆ 中等 / ★★★ 困难 |

### 📌 优先级总览（时间不够时按此取舍）

| 优先级 | 内容 | 说明 |
|---|---|---|
| 🔴 P0 | 阶段二全部（手写三大范式）、阶段四全部（自研框架）、8A Memory/RAG、8B 上下文工程、8C MCP | 全书主干，共约 8 周 |
| 🟡 P1 | 阶段一、阶段三、Skills/工程化小练习、最低评估门槛、阶段六综合项目、毕业设计 | 建立认知与可验证的作品，压缩时先砍深度不砍覆盖 |
| 🟢 P2 | 完整 BFCL/GAIA、Agentic-RL、1.x 迁移、n8n/AgentScope/CAMEL、GUI/Web Agent、自进化 | 按方向选学；基本评估不再与训练二选一 |

### 环境与版本约定（开始第 7 章前必读）

本仓库是**课程文档 + 分章示例 + 社区项目合集**，框架源码主要在另一个 [HelloAgents 仓库](https://github.com/jjyaoao/HelloAgents)。安装分发名是 **`hello-agents`**，Python 导入名是 **`hello_agents`**；不要沿用旧计划中的 `pip install helloagents`。

| 范围 | 材料中的版本依据 | 执行约定 |
|---|---|---|
| 第 7 章基础框架 | 正文固定 `hello-agents==0.1.1` | 先建 `.venv-ch7`，按正文版本学习继承与工具；这不是最新框架 |
| 第 7 章 7.4.5 原生 Function Calling | 正文要求后期 0.2.x；0.2.9 wheel 中存在 `FunctionCallAgent` | 单独建 `.venv-ch7-fc`，安装 `"hello-agents==0.2.9"`；不在 0.1.1 中找这个类 |
| 第 8~10 章 | 第 8 章标 0.2.0（附 0.2.9 排障建议），第 9 章标 0.2.8，第 10 章标 0.2.2 | 按章隔离、按正文配置；第 10 章正文 extra 写成 `protocol`，安装前核对对应版本实际提供的 extras，不能照搬到所有版本 |
| 第 12 章完整基准 | 代码 README 写 0.2.3，正文写 0.2.7，另有 NumPy 1.26.4 示例 | 材料存在版本不一致，放独立评估环境排障；0.2.9 元数据要求 NumPy >=2，不能把这些命令混装在主线环境 |
| 第 13~15 章 | 第 13/15 章约束 `>=0.2.4,<=0.2.9`；第 14 章固定 0.2.9、有 `uv.lock` | 各项目按自己的依赖文件安装；第 14 章优先 `uv sync --frozen` |
| 框架新版本与社区项目 | 2026-10-26 查得最新发布版为 1.0.1，Python `>=3.12,<3.14` | 与 0.x 教程分开；社区项目分别查看 Python 和框架约束，不统一升级 |

每个新环境先运行一个最小示例，再保存 `python --version`、包版本、`python -m pip check` 结果和依赖快照。**上述表是材料及包元数据核对结果，不代表所有示例已在本机联网跑通。** 遇到 API 差异先查版本，不要用全局升级掩盖问题。

### ⚠️ 易卡点预警（提前准备，避免中断）

| 卡点 | 出现位置 | 提前准备 |
|---|---|---|
| API Key 配置 | 阶段一 D3 起，贯穿全程 | 第 1 天就配好环境变量，参照 [Extra07-环境配置.md](Extra-Chapter/Extra07-环境配置.md) |
| 搜索 API 配置 | 阶段二 ReAct 起；阶段四联网工具继续使用 | 第 4 章 `tools.py` 使用 `SERPAPI_API_KEY`；第 7 章按自己的搜索工具配置，不同服务的 Key 不能互换 |
| 包名与版本漂移 | 阶段四及以后 | 分发名 `hello-agents`，导入名 `hello_agents`；0.x 教程与 1.x 环境隔离 |
| Dify / n8n 账号与安装 | 阶段三 D1~D3 | 云端版提前注册；本地版预留半天装环境 |
| A2A 需先起 Server | 阶段五 8C D4~D5 | 开两个终端，先跑 `09_A2A_Server.py` 再跑 Client |
| GPU 算力 | 核心路线之后的 Agentic-RL 选学（第 11 章） | 无 GPU 先读数据、reward 与训练流程，不影响应用主线 |
| 前后端双服务 + 端口冲突 | 阶段六选项 A | 学会同时管理两个进程，注意端口占用 |

## 📑 目录

- [阶段一：认知奠基（第 1~3 章）- 第 1 周](#阶段一认知奠基第-1~3-章--第-1-周)
- [阶段二：手写经典范式（第 4 章）- 第 2~3 周 ⭐](#阶段二手写经典范式第-4-章--第-2~3-周-最高优先级)
- [阶段三：生态广度（第 5~6 章）- 第 4 周](#阶段三生态广度第-5~6-章--第-4-周快速通过)
- [阶段四：构建自己的框架（第 7 章）- 第 5~6 周 ⭐](#阶段四构建自己的框架第-7-章--第-5~6-周-全书核心)
- [阶段五：高级能力（第 8~12 章）- 第 7~9 周](#阶段五高级能力第-8~12-章--第-7~9-周)
- [阶段六：综合实战（第 13~16 章）- 第 10~12 周](#阶段六综合实战第-13~16-章--第-10~12-周)
- [贯穿全程的三条线](#贯穿全程的三条线)
- [里程碑检查点汇总](#里程碑检查点汇总)
- [弹性调整建议](#弹性调整建议)
- [本次更新补充清单](#本次更新补充清单)

---

## 阶段一：认知奠基（第 1~3 章）- 第 1 周 🟡 P1

**阶段目标**：能说清 Agent 的定义与范式分类；理解 LLM 的 token 化与基本原理，为后续章节扫清术语障碍。

### 第 1 周

- [x] **D1~D2｜概念框架**【P1｜重点｜★☆☆】
  - 动作：通读 [docs/chapter1/第一章 初识智能体.md](docs/chapter1/第一章%20初识智能体.md)，重点记录：①智能体定义 ②"AI 原生 Agent vs 软件工程类 Agent"的分类 ③Agent 的核心组成（感知-决策-行动）
  - 验收：不看资料，用 3 句话向别人解释"什么是 Agent、Agent 和 Chatbot 的本质区别"

- [x] **D3｜第一个 Agent + 发展史**【P1｜重点｜★☆☆】
  - 动作：①跑通 [code/chapter1/FirstAgentTest.py](code/chapter1/FirstAgentTest.py)（先配置 API Key 环境变量）②读 [docs/chapter2/第二章 智能体发展史.md](docs/chapter2/第二章%20智能体发展史.md)
  - 验收：`FirstAgentTest.py` 输出正常回复；能按时间线说出符号主义 -> 连接主义 -> LLM 驱动智能体的 3 个关键转折点

- [x] **D4｜历史对比实验**【P2｜★☆☆】
  - 动作：跑通 [code/chapter2/ELIZA.py](code/chapter2/ELIZA.py)，用同一句话（如"我最近很焦虑"）分别问 ELIZA 和 Day1 的 LLM Agent
  - 产出：一段对比笔记（ELIZA 靠规则模板，LLM Agent 靠概率生成），存入个人笔记

- [x] **D5~D6｜语言模型基础**【P1｜★★☆】
  - 动作：读 [docs/chapter3/第三章 大语言模型基础.md](docs/chapter3/第三章%20大语言模型基础.md)；依次跑 [code/chapter3/](code/chapter3/) 下 [BPE.py](code/chapter3/BPE.py)、[N_gram.py](code/chapter3/N_gram.py)、[Word_Embedding.py](code/chapter3/Word_Embedding.py)，观察各自输入输出
  - 验收：能回答"为什么模型按 token 计费而不是按字？"、"N-gram 和神经语言模型的区别？"

- [x] **D7｜Transformer 与收尾**【P2｜难点｜★★★】
  - 动作：跑 [Transformer.py](code/chapter3/Transformer.py)；手画一张 Self-Attention 数据流草图（Q/K/V -> 打分 -> 加权求和）
  - 验收：草图能在 30 秒内讲清楚 Attention "查询-匹配-聚合"的三步
  - 💡 数学推导是本书最难的部分，但**只需理解直觉，不必推导**，卡住就先跳过

> 💡 第 3 章的数学推导不要求掌握，目标是读得懂后续章节出现的模型术语即可。**已有 LLM 基础者可整段跳过本阶段。**

---

## 阶段二：手写经典范式（第 4 章）- 第 2~3 周 🔴 P0 ⭐ 最高优先级

**阶段目标**：脱离任何框架，纯手写实现 ReAct / Plan-and-Solve / Reflection 三大范式，理解"Agent = LLM + 工具 + 循环"的本质。
**为何 P0**：后面所有框架（LangGraph、AutoGen、HelloAgents）本质上都在封装这三个范式，这是全书的绝对地基。

### 第 2 周

- [ ] **D1｜最小骨架**【P0｜重点｜★★☆】
  - 动作：读 [docs/chapter4/第四章 智能体经典范式构建.md](docs/chapter4/第四章%20智能体经典范式构建.md) 前半部分；逐行精读 [code/chapter4/llm_client.py](code/chapter4/llm_client.py) 和 [tools.py](code/chapter4/tools.py)
  - 验收：能指出 `tools.py` 中工具的"描述（给 LLM 看）"和"实现（给 Python 看）"分别是哪部分、为什么两者都要有

- [ ] **D2~D4｜手敲 ReAct**【P0｜重点+难点｜★★★】（**全书最关键任务**）
  - 动作：**逐行手敲**（禁止复制粘贴）[code/chapter4/ReAct.py](code/chapter4/ReAct.py)，边敲边写自己的注释；原版只注册 Search，先跑通 2 个搜索任务，数学计算工具在改造练习中自行添加
  - 验收：合上代码，白板写出 ReAct 主循环伪代码（含 Thought 解析、Action 执行、Observation 拼接、终止条件判断）
  - ⚠️ 难点在 **LLM 输出解析**（从自由文本里稳定提取 Thought/Action），这是最容易卡壳的地方，解析失败时优先检查 prompt 格式约束

- [ ] **D5~W3 D1｜手敲 Plan-and-Solve**【P0｜重点｜★★☆】
  - 动作：同样方式手敲 [Plan_and_solve.py](code/chapter4/Plan_and_solve.py)，注意对比它和 ReAct 的 prompt 结构差异
  - 验收：能说清"先规划后执行"在代码里对应哪几行；两种范式各自适合什么任务

### 第 3 周

- [ ] **D2~D3｜手敲 Reflection**【P0｜重点｜★★☆】
  - 动作：手敲 [Reflection.py](code/chapter4/Reflection.py)，跑通并观察 2~3 轮自我改进的输出变化
  - 验收：能回答"反思环节的输入是什么、失败时如何止损（最大轮数）"

- [ ] **D4~D5｜改造练习：加一个自己的工具**【P0｜★★☆】
  - 动作：在 [tools.py](code/chapter4/tools.py) 风格基础上，给 ReAct Agent 新增一个工具（三选一：天气查询、汇率换算、随机名言），并跑通一次完整的 Thought->Action->Observation
  - 产出：`my_react_agent.py`（自定义版本，放入个人练习目录）

- [ ] **D6~D7｜范式对比实验**【P1｜重点｜★★☆】
  - 动作：先确认三者能力边界：原版 ReAct 有 Search，Plan-and-Solve 主要做 LLM 分步求解，Reflection 的提示词针对代码改进。分别记录适配任务的成功/失败案例；需要同题比较时，统一工具、模型和验收标准后再对比
  - 产出：一篇对比笔记（什么场景用 ReAct、什么场景用 Plan-and-Solve、Reflection 何时有价值），附工具可用性与调用成本，避免把工具差异当成范式优劣
  - 视频辅助：卡在循环或提示词时，参考 [Extra13 视频导航](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md) 的第 4 章视频，观看后仍需自行实现
  - 💡 该笔记是面试高频题「三种范式如何选型」的现成答案

> ⛔ **检查点（第 3 周末）**：白板手写 ReAct 循环不卡壳 -> 进入阶段三；卡壳 -> 用 2 天重过 D2~D4，不允许带病前进。

---

## 阶段三：生态广度（第 5~6 章）- 第 4 周 🟡 P1（快速通过）

**阶段目标**：体验低代码平台与主流代码框架，建立"框架封装了什么"的判断力，不追求精通。

- [ ] **D1~D2｜Coze / Dify 二选一**【P2｜★☆☆】
  - 动作 A：按更新后的 [第 5 章 5.2 节](docs/chapter5/第五章%20基于低代码平台的智能体搭建.md) 和 [Extra13 中 Coze 26.10 月版视频](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md) 搭建每日 AI 简报，重点理解新版入口、工作流资源、插件输入输出、系统/用户提示词
  - 动作 B：注册 Dify（云端或本地部署）；导入 [code/chapter5/HelloAgent_difyCase.yml](code/chapter5/HelloAgent_difyCase.yml)，与教程对照理解每个节点。完成 A 或 B 即可，不叠加两天任务
  - 验收：能改一个 prompt 节点并重新跑通；能解释流程编排与自主工具选择的区别。UI 以当前实际入口为准，不把找到同款截图作为学习目标
  - 加餐（可选）：[Extra-Chapter/Extra03-Dify智能体创建保姆级操作流程.md](Extra-Chapter/Extra03-Dify智能体创建保姆级操作流程.md)

- [ ] **D3｜n8n 体验**【P2｜★☆☆】
  - 动作：按 [Additional-Chapter/N8N_INSTALL_GUIDE.md](Additional-Chapter/N8N_INSTALL_GUIDE.md) 安装 n8n，导入 [code/chapter5/HelloAgent_n8nCase.json](code/chapter5/HelloAgent_n8nCase.json)
  - 验收：跑通一次自动化流程，笔记记录 n8n 与 Dify 的定位差异
  - 💡 n8n 安装耗时不确定，卡住就直接放弃，不影响主线

- [ ] **D4~D5｜LangGraph（重点）**【P1｜重点｜★★☆】
  - 动作：安装依赖（[code/chapter6/Langgraph/requirements.txt](code/chapter6/Langgraph/requirements.txt)），跑通 [Dialogue_System.py](code/chapter6/Langgraph/Dialogue_System.py)；**对照第 4 章手写代码**回答：State / Node / Edge 分别对应我手写 ReAct 里的哪个变量/函数？
  - 产出：一张"手写代码 ↔ LangGraph 概念"对照表（这是本阶段最重要的产出，也是面试考点）
  - 视频辅助：[Extra13 第 6 章 LangGraph 跟练实战](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md)

- [ ] **D6｜AutoGen 多智能体**【P1｜重点｜★★☆】
  - 动作：装好依赖后跑通 [code/chapter6/AutoGenDemo/autogen_software_team.py](code/chapter6/AutoGenDemo/autogen_software_team.py)，观察多个 Agent 角色如何对话协作
  - 验收：能说出这个软件团队 Demo 里有哪些角色、谁负责终止对话

- [ ] **D7｜AgentScope / CAMEL 速览 + 选型笔记**【P2｜★☆☆】
  - 动作：跑通 [code/chapter6/AgentScopeDemo/main_cn.py](code/chapter6/AgentScopeDemo/main_cn.py) 与 [code/chapter6/CAMEL/DigitalBookWriting.py](code/chapter6/CAMEL/DigitalBookWriting.py)（任选其一深入亦可）
  - 产出：半页框架选型笔记（LangGraph / AutoGen / AgentScope / CAMEL 各自适用场景）

> 💡 本阶段只有 LangGraph 和 AutoGen 是 P1，低代码平台和其余框架都是 P2，时间紧可只跑这两个。

---

## 阶段四：构建自己的框架（第 7 章）- 第 5~6 周 🔴 P0 ⭐ 全书核心

**阶段目标**：基于自研 HelloAgents 框架，掌握"继承基类 -> 注册工具 -> 多 Agent 编排"的完整框架开发方法，最终独立扩展框架。
**为何 P0**：这是全书唯一教你"框架内部长什么样"的章节，是从使用者到构建者的分水岭。

### 第 5 周

- [ ] **D1｜框架总览**【P0｜★☆☆】
  - 动作：建立本章独立虚拟环境，执行 `python -m pip install "hello-agents==0.1.1"`（详见 [第 7 章](docs/chapter7/第七章%20构建你的Agent框架.md) 开头）；通读该章文档，画出框架类层次草图（SimpleAgent / ReActAgent / PlanAndSolveAgent / ReflectionAgent / ToolRegistry / HelloAgentsLLM 的关系）
  - 验收：草图能说清"哪个类负责什么、谁继承谁"

- [ ] **D2~D3｜理解基类与重写**【P0｜重点｜★★☆】
  - 动作：精读 [code/chapter7/my_simple_agent.py](code/chapter7/my_simple_agent.py)（重点：`__init__` 参数、`run()` 的消息组装逻辑、`_get_enhanced_system_prompt()`），跑通 [test_simple_agent.py](code/chapter7/test_simple_agent.py)
  - 验收：能指出子类重写了父类的哪些行为、为什么要重写
  - 更新核对：本次上游移除了 `stream_run()` 中重复打印 chunk 的一行；检查 `HelloAgentsLLM.stream_invoke()` 和调用方各自是否输出，确认每个 chunk 只显示一次。不能只根据 `test_simple_agent.py` 里的旧注释判断打印职责

- [ ] **D4~D5｜ReAct Agent 与第一个自定义工具**【P0｜重点+难点｜★★★】
  - 动作：精读 [my_react_agent.py](code/chapter7/my_react_agent.py)，对照第 4 章手写版找"框架帮你做了什么"；精读 [my_calculator_tool.py](code/chapter7/my_calculator_tool.py) 的工具定义规范（名称/描述/参数 schema/执行方法）；跑通 [test_react_agent.py](code/chapter7/test_react_agent.py) 和 [test_my_calculator.py](code/chapter7/test_my_calculator.py)
  - 验收：能独立说出"定义一个新工具必须实现哪几样东西"
  - ⚠️ 难点是读懂父类源码（需点进 `hello_agents` 包里看），建议直接阅读安装目录下的框架源码

### 第 6 周

- [ ] **D1~D2｜第二个工具：联网搜索**【P0｜难点｜★★☆】
  - 动作：精读 [my_advanced_search.py](code/chapter7/my_advanced_search.py)，跑通 [test_advanced_search.py](code/chapter7/test_advanced_search.py)；解决其中的搜索 API 配置
  - 验收：Agent 能针对一个时事问题自主决定调用搜索并给出带来源的回答
  - ⚠️ 卡点：需要提前注册搜索服务拿 API Key（见顶部易卡点预警）

- [ ] **D3｜原生 Function Calling 与 Provider 扩展**【P0｜重点｜★★☆】
  - 动作：在独立 `.venv-ch7-fc` 中安装 `"hello-agents==0.2.9"`，学习第 7 章 **7.4.5 FunctionCallAgent**，用同一个计算工具对照文本 `[TOOL_CALL:...]` 解析和原生 `tool_calls`，检查 JSON 参数、工具结果回填与最大轮数
  - 验收：保存一条“模型请求工具 -> 执行工具 -> 回填结果 -> 最终回答”的轨迹；说明原生调用仍需参数验证与失败处理
  - Provider 速览：读 [my_llm.py](code/chapter7/my_llm.py) 和 [my_main.py](code/chapter7/my_main.py)。后者是 **ModelScope 客户端示例，不是多 Agent 编排**；没配 ModelScope Key 时只读即可，不额外开服务账号

- [ ] **D4｜自己实现最小双 Agent 编排**【P0｜重点+难点｜★★★】
  - 动作：在个人练习目录写 planner -> executor 顺序工作流，用已学 Agent 组成两个角色，显式把第一个的输出传给第二个；参考第 6 章协作思路，不把两个独立调用当成协作
  - 验收：画出任务、共享状态/中间结果与终止条件的时序图；交换一次角色配置观察结果
  - 补全练习（P2）：[test_plan_solve_agent.py](code/chapter7/test_plan_solve_agent.py) 和 [test_reflection_agent.py](code/chapter7/test_reflection_agent.py) 依赖的 `my_plan_solve_agent.py` / `my_reflection_agent.py` 没有随仓库提供，需要从正文实现，不能作为开箱即跑脚本

- [ ] **D5~D7｜阶段大作业**【P0｜★★☆】
  - 动作：构建"XX 领域助手"：①继承一个 Agent 基类 ②实现 2 个自定义工具（其中 1 个全新，不能照抄教程）③用 2 个 Agent 协作（如 planner + executor）④写 README
  - 产出：一个可演示的完整 Demo 目录（建议：学习助手 / 健身教练 / 美食推荐任选）

> ⛔ **检查点（第 6 周末）**：能不看教程加工具、解释原生工具调用、完成 planner -> executor 真实传参 -> 进入阶段五；不能 -> 重做对应练习。

---

## 阶段五：高级能力（第 8~12 章）- 第 7~9 周

**阶段目标**：掌握 Memory、上下文工程、MCP，补上 Skills 与工程化基础，并完成一次可重复的最小评估。第 8 周留给上下文工程，第 9 周安排通信与评估，避免原计划“第 8 周前/后半”各排 5 天的冲突。

### 8A. Memory 与 RAG（第 8 章）- 第 7 周 🔴 P0 ✅ 必修

- [ ] **D1~D3｜记忆系统**【P0｜重点｜★★☆】
  - 动作：先跑 [01_MemoryTool_Basic_Operations.py](code/chapter8/01_MemoryTool_Basic_Operations.py)，精读 [02_MemoryTool_Architecture.py](code/chapter8/02_MemoryTool_Architecture.py)、[03_WorkingMemory_Implementation.py](code/chapter8/03_WorkingMemory_Implementation.py)；[06_Memory_Consolidation_Demo.py](code/chapter8/06_Memory_Consolidation_Demo.py)、[09_Memory_Types_Deep_Dive.py](code/chapter8/09_Memory_Types_Deep_Dive.py) 按时间选跑
  - 验收：能区分工作记忆 / 短期记忆 / 长期记忆在代码里的实现差异，说出"记忆固化"解决什么问题
  - 视频辅助：[Extra13 第 8 章记忆视频与配套课件入口](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md)，当前导航收录前两节视频，不能当作整章 RAG 的视频替代

- [ ] **D4~D6｜RAG 管道**【P0｜重点+难点｜★★★】
  - 动作：依次跑 [04_RAGTool_MarkItDown_Pipeline.py](code/chapter8/04_RAGTool_MarkItDown_Pipeline.py)（文档解析）-> [05_RAGTool_Advanced_Search.py](code/chapter8/05_RAGTool_Advanced_Search.py)（检索）-> [07_RAGTool_Intelligent_QA.py](code/chapter8/07_RAGTool_Intelligent_QA.py)（问答）-> [10_RAG_Pipeline_Complete.py](code/chapter8/10_RAG_Pipeline_Complete.py)（完整管道）
  - 验收：能画出 RAG 五阶段流程图（加载->切分->向量化->检索->生成），并指出每个脚本对应哪一阶段
  - ⚠️ 难点是向量检索的质量调优（切分粒度、top-k、相似度阈值），初学先跑通，调优概念了解即可

- [ ] **D7｜综合应用**【P0｜★★☆】
  - 动作：从 [11_Q&A_Assistant.py](code/chapter8/11_Q&A_Assistant.py) 和 [08_Agent_Tool_Integration.py](code/chapter8/08_Agent_Tool_Integration.py) 中选一个运行；**给阶段四的大作业加上长期记忆**（让助手记住用户偏好），未选示例先读代码
  - 产出：升级版大作业；记录一次进程重启后读取偏好的结果，避免把当前对话历史误当成持久记忆

### 8B. 上下文工程（第 9 章）- 第 8 周 🔴 P0 ✅ 必修

- [ ] **D1~D2｜Context Builder**【P0｜重点｜★★☆】
  - 动作：跑 [code/chapter9/](code/chapter9/) 的 [01_context_builder_basic.py](code/chapter9/01_context_builder_basic.py) -> [02_context_builder_with_agent.py](code/chapter9/02_context_builder_with_agent.py)，理解上下文如何被结构化组装
  - 验收：能说出"上下文工程 ≠ 塞更多 token"的本质（信息的选择与组织）

- [ ] **D3~D4｜工具集成**【P1｜★★☆】
  - 动作：跑 [03_note_tool_operations.py](code/chapter9/03_note_tool_operations.py)、[04_note_tool_integration.py](code/chapter9/04_note_tool_integration.py)、[05_terminal_tool_examples.py](code/chapter9/05_terminal_tool_examples.py)
  - 验收：理解 Agent 如何通过工具把笔记/终端操作纳入上下文

- [ ] **D5｜综合工作流**【P1｜难点｜★★★】
  - 动作：跑 [06_three_day_workflow.py](code/chapter9/06_three_day_workflow.py)，对照 [code/chapter9/README.md](code/chapter9/README.md) 理解三日工作流设计；快速浏览 [codebase/](code/chapter9/codebase/)（api_client / data_processor 等模块）理解 [codebase_maintainer.py](code/chapter9/codebase_maintainer.py) 如何维护项目上下文
  - 加餐：[Extra-Chapter/Extra02-上下文工程补充知识.md](Extra-Chapter/Extra02-上下文工程补充知识.md)
  - 验收：能解释"为什么 Claude Code 这类产品强依赖上下文工程"
  - ⚠️ 难点是跨多天的状态持久化设计，抓住"什么该进上下文、什么该存盘"这条主线即可
  - 视频辅助：[Extra13 第 9 章上下文工程讲解与实战](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md)

- [ ] **D6~D7｜Skills 与可观测性小练习**【P1｜重点｜★★☆】
  - 动作：读 [Extra05 Agent Skills 解读](Extra-Chapter/Extra05-AgentSkills解读.md)、[Extra08 如何写出好的 Skill](Extra-Chapter/Extra08-如何写出好的Skill.md)，把自己的一个固定工作流写成 `SKILL.md`（触发条件、输入、步骤、产物、失败处理）；再选读 [Extra09 工程实践](Extra-Chapter/Extra09-Agent应用开发实践踩坑与经验分享.md) 的工具、上下文与 Trace 部分
  - 实践：在练习应用中为一次运行记录任务 ID、工具名称/参数、错误、耗时和调用次数；用手动加载/显式注入方式验证 Skill，课程 0.x 不预设有自动 Skills 加载器
  - 验收：能区分 Skill（工作方法）、MCP（工具接入协议）、上下文（本轮提供的信息）；能用一条失败轨迹解释错误发生在哪一步

### 8C. 通信协议 MCP / A2A（第 10 章）- 第 9 周 D1~D5 🔴 P0 ✅ 必修

- [ ] **D1~D2｜MCP 基础**【P0｜重点｜★★☆】
  - 动作：依次跑 [code/chapter10/](code/chapter10/) 的 [01_TestConnect.py](code/chapter10/01_TestConnect.py) -> [02_Connect2MCP.py](code/chapter10/02_Connect2MCP.py) -> [03_GitHubMCP.py](code/chapter10/03_GitHubMCP.py) -> [04_MCPTransport.py](code/chapter10/04_MCPTransport.py)
  - 验收：能说清 MCP 的 Client / Server / Transport 三要素；`03` 先用 GitHub 读取操作验证工具调用，需要写入时仅在自己的测试仓库练习

- [ ] **D3｜MCP 进阶**【P1｜重点｜★★☆】
  - 动作：跑 [05_UseMCPToolInAgent.py](code/chapter10/05_UseMCPToolInAgent.py)、[06_MultiAgentDocumentAssist.py](code/chapter10/06_MultiAgentDocumentAssist.py)（MCP + 多智能体文档助手）
  - 验收：理解 MCP 工具如何被注册进 Agent 的工具体系

- [ ] **D4~D5｜A2A 协议**【P1｜难点｜★★★】
  - 动作：先读 [07_SimpleA2AAgent.py](code/chapter10/07_SimpleA2AAgent.py)，再跑 [09_A2A_Server.py](code/chapter10/09_A2A_Server.py) + [09_A2A_Client.py](code/chapter10/09_A2A_Client.py)（先起 Server 再起 Client）；[08_CustomA2AAgent.py](code/chapter10/08_CustomA2AAgent.py)、[09_A2A_Network.py](code/chapter10/09_A2A_Network.py)、[09_A2A_WithAgent.py](code/chapter10/09_A2A_WithAgent.py) 先读后选跑
  - 验收：能对比 MCP（Agent↔工具）与 A2A（Agent↔Agent）解决的问题差异
  - ⚠️ 卡点：需两个终端分别跑 Server 和 Client，顺序颠倒会连接失败（见顶部预警）
  - 回顾第 8 周 Skill 练习，解释“写操作步骤”和“开放工具能力”分别解决什么问题

### 8D. 最低评估门槛 - 第 9 周 D6~D7 🟡 P1

- [ ] **D6｜固定自己的小测试集**【P1｜重点｜★★☆】
  - 动作：读第 12 章评估指标与 [01_basic_agent_example.py](code/chapter12/01_basic_agent_example.py)，给阶段四大作业写 5~10 个固定用例，覆盖正常调用、参数错误、工具失败、最大轮数、跨会话偏好；用例包含明确的预期结果或判定规则
- [ ] **D7｜改动前后对比**【P1｜重点｜★★☆】
  - 动作：同一模型、工具与测试集比较一次修改前后结果；保留成功率及调用次数/耗时两类指标，并记录至少 2 个失败案例
  - 验收：一份可重跑的评估报告；单纯“看起来更聪明”不算通过。可先用本地假工具检验失败分支，不以下载完整 BFCL/GAIA 为前提

### 方向选学 - 核心路线之后 🟢 P2（不挤占第 9 周）

**路线 A：完整评估 -> 第 12 章 BFCL / GAIA**【P2｜重点(做应用方向)｜★★☆】（另排约 1 周）
- [ ] D1~D2：跑 [code/chapter12/](code/chapter12/) 的 [01_basic_agent_example.py](code/chapter12/01_basic_agent_example.py)、[02_bfcl_quick_start.py](code/chapter12/02_bfcl_quick_start.py)，理解 BFCL 工具调用评估
- [ ] D3：跑 [03_bfcl_custom_evaluation.py](code/chapter12/03_bfcl_custom_evaluation.py)、[04_run_bfcl_evaluation.py](code/chapter12/04_run_bfcl_evaluation.py)，给自己的大作业写一次评估
- [ ] D4~D5：跑 [05_gaia_quick_start.py](code/chapter12/05_gaia_quick_start.py)、[06_gaia_best_practices.py](code/chapter12/06_gaia_best_practices.py)
- [ ] D6~D7：跑 `07`~`09` 数据生成脚本（[07_data_generation_complete_flow.py](code/chapter12/07_data_generation_complete_flow.py)、LLM-as-Judge、胜率对比），理解"没有测试集就自己造"
- 验收：给自己的大作业跑出一份评估报告（含至少 2 个指标）

**路线 B：模型训练 -> 第 11 章 Agentic-RL**【P2｜难点(全书最难)｜★★★】（另排 1~2 周，需 GPU 或云算力）
- [ ] D1：环境准备（trl / peft / accelerate），跑 [code/chapter11/00_quick_test.py](code/chapter11/00_quick_test.py) 验证环境
- [ ] D2：跑 [01_dataset_loading.py](code/chapter11/01_dataset_loading.py)，理解数据格式
- [ ] D3：精读 [02_reward_functions.py](code/chapter11/02_reward_functions.py)，能自己写一个简单 reward
- [ ] D4：跑 [03_lora_configuration.py](code/chapter11/03_lora_configuration.py)，理解 LoRA 参数量与显存的关系
- [ ] D5~D7：按顺序跑 [04_sft_training.py](code/chapter11/04_sft_training.py) -> [05_grpo_training.py](code/chapter11/05_grpo_training.py) -> [06_complete_pipeline.py](code/chapter11/06_complete_pipeline.py)
- [ ] 加餐：[07_model_evaluation.py](code/chapter11/07_model_evaluation.py)、[08_distributed_training.py](code/chapter11/08_distributed_training.py)（参考 [accelerate_configs/](code/chapter11/accelerate_configs/) 三种分布式配置）
- 加餐：[Extra12 旅行助手后训练实战](Extra-Chapter/Extra12-旅行助手后训练实战.md)：先读“产品协议 -> 冻结评测集 -> Prompt -> 训练 -> Rerank”的路线，再决定是否做 SFT/DPO；不要把 DPO 和本章 GRPO 当作同一训练任务
- ⚠️ 全书工程量与算力门槛最高的部分：reward 设计（D3）是核心难点，显存不足时优先降 LoRA rank 和序列长度

> ✅ **检查点（第 9 周末）**：Memory、上下文工程、MCP + 一份最小评估报告完成，再进入阶段六；完整基准和训练可后补。

---

## 阶段六：综合实战（第 13~16 章）- 第 10~12 周 🟡 P1

**阶段目标**：深入一个完整项目并做出自己的修改，最终完成可开源的毕业设计。

### 第 10~11 周：三选一深入（不许只跑 demo，必须改一个模块）

- [ ] **选项 A：智能旅行助手**【P1｜重点+难点｜★★★】（适合想做全栈产品）
  - 启动：读 [code/chapter13/helloagents-trip-planner/README.md](code/chapter13/helloagents-trip-planner/README.md)；后端：在 [backend/](code/chapter13/helloagents-trip-planner/backend/) 下执行 `pip install -r requirements.txt`，再运行 `python run.py`；前端在 [frontend/](code/chapter13/helloagents-trip-planner/frontend/) 下用 npm 启动（Vite 项目，见 [package.json](code/chapter13/helloagents-trip-planner/frontend/package.json)）
  - 理解：跑通后画出后端 [app/](code/chapter13/helloagents-trip-planner/backend/app/) 内的 Agent 编排图与 MCP 工具调用链
  - **改造任务（必做其一）**：①换一个 Planner 策略（如从 Plan-and-Solve 换成 Reflection）②新增一个 MCP 工具（如天气、美食）③调整多 Agent 分工
  - ⚠️ 卡点：前后端双服务 + 端口配置，建议先用 README 的默认端口跑通再改

- [ ] **选项 B：DeepResearch Agent**【P1｜难点｜★★★】（适合想做研究类应用）
  - 启动：[code/chapter14/helloagents-deepresearch/backend](code/chapter14/helloagents-deepresearch/backend) 是 uv 项目（优先 `uv sync --frozen` 使用现有锁文件，见 [pyproject.toml](code/chapter14/helloagents-deepresearch/backend/pyproject.toml)）；跑通一个研究任务
  - 理解：画出"规划 -> 并行检索 -> 综合写作"的完整链路
  - **改造任务（必做其一）**：①替换/增加一个检索源 ②调整报告生成的章节规划策略 ③加引用溯源校验
  - ⚠️ 卡点：需先安装 uv（非 pip）；完整研究任务耗时长、token 消耗大，调试时用小问题
  - 视频辅助：[Extra13 第 14 章 DeepResearch 视频](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md)

- [ ] **选项 C：赛博小镇 AI-Town**【P1｜★★☆】（适合对多智能体模拟感兴趣）
  - 启动：读 [code/chapter15/Helloagents-AI-Town/SETUP_GUIDE.md](code/chapter15/Helloagents-AI-Town/SETUP_GUIDE.md)、[README.md](code/chapter15/Helloagents-AI-Town/README.md)；跑通 backend（注意 [agents.py](code/chapter15/Helloagents-AI-Town/backend/agents.py)、[relationship_manager.py](code/chapter15/Helloagents-AI-Town/backend/relationship_manager.py)、`memory_data/`）
  - 理解：结合 [MEMORY_SYSTEM_GUIDE.md](code/chapter15/Helloagents-AI-Town/MEMORY_SYSTEM_GUIDE.md)、[AFFINITY_SYSTEM_GUIDE.md](code/chapter15/Helloagents-AI-Town/AFFINITY_SYSTEM_GUIDE.md)、[DIALOGUE_LOG_GUIDE.md](code/chapter15/Helloagents-AI-Town/DIALOGUE_LOG_GUIDE.md) 三份文档，说清记忆系统、亲和度系统、对话日志的运作方式
  - **改造任务（必做其一）**：①新增一个 Agent 角色并观察社会动态变化 ②调整亲和度规则做一次"社会学实验" ③为小镇增加一个事件系统

### 第 12 周：毕业设计

- [ ] **D1~D2｜选题与设计**【P1｜★★☆】
  - 动作：读 [docs/chapter16/第十六章 毕业设计.md](docs/chapter16/第十六章%20毕业设计.md) + [code/chapter16/共创路径.md](code/chapter16/共创路径.md)；从 [Co-creation-projects/](Co-creation-projects/) 浏览 3~5 个社区项目找灵感（推荐 [jjyaoao-CodeReviewAgent](Co-creation-projects/jjyaoao-CodeReviewAgent)、[chengH425-PaperAssistant](Co-creation-projects/chengH425-PaperAssistant)、[zjzhou-SREOnCallAgent](Co-creation-projects/zjzhou-SREOnCallAgent)）
  - 产出：一页设计文档（要解决什么问题、用哪些技术、Agent 如何分工）
  - 本次新增案例：优先读 [JobMatchAgent](Co-creation-projects/junqiangyuan-JobMatchAgent/README.md) 的三阶段协作与证据校验，或 [FrameCraft](Co-creation-projects/Loveiuer-FrameCraft/README.md) 的结构化产物与确定性生成；研究方向再看 [PaperMind](Co-creation-projects/jphJack-PaperMindAgent/README.md) / [PaperAssistant](Co-creation-projects/Lemoween-PaperAssistant/README.md)。这些案例替换原来的 3~5 个浏览名额，不再额外全跑一遍；每个项目按自己的版本建环境

- [ ] **D3~D6｜开发**【P1｜难点｜★★★】
  - 硬性要求：必须包含 ①长期 Memory ②至少 1 个 MCP 工具或自定义工具 ③至少 2 个 Agent 协作
  - 技术栈建议直接复用：HelloAgents 框架（第 7 章）+ Memory/RAG（第 8 章）+ 上下文工程（第 9 章）
  - ⚠️ 难点不是写代码而是收敛范围：设计文档阶段就把功能砍到 4 天能完成的最小集合

- [ ] **D7｜收尾开源**【P1｜★☆☆】
  - 产出：完整 README（含架构图、Python/依赖版本、运行方式、演示截图/GIF、复用第 9 周测试集的回归报告），推送到自己的 GitHub 仓库

---

## 贯穿全程的三条线

| 线 | 做法 | 频率 |
|---|---|---|
| **面试线** | 完成一个阶段后，做 [Extra-Chapter/Extra01-面试问题总结.md](Extra-Chapter/Extra01-面试问题总结.md) 对应章节的题，对照 [Extra01-参考答案.md](Extra-Chapter/Extra01-参考答案.md) 查漏 | 每阶段一次 |
| **笔记线** | 周日花 30 分钟写周记：学到什么 / 卡在哪 / 下周计划；12 周后即个人 Agent 知识库 | 每周一次 |
| **答疑线** | 卡住先查 [Extra-Chapter/Extra04-DatawhaleFAQ.md](Extra-Chapter/Extra04-DatawhaleFAQ.md) -> 查仓库 Issues -> 提 Issue 或进读者群（[README.md](README.md) 有二维码） | 随时 |

## 里程碑检查点汇总

| 时间点 | 检查内容 | 不达标动作 |
|---|---|---|
| 第 1 周末 | 能解释 Agent 定义与范式分类 | 重读第 1 章 |
| 第 3 周末 | 白板手写 ReAct 主循环 | 重过阶段二 D2~D4 |
| 第 4 周末 | 完成"手写代码 ↔ LangGraph"对照表 | 重跑 LangGraph Demo |
| 第 6 周末 | 能加工具、解释原生调用、完成双 Agent 传参 | 重过阶段四对应练习 |
| 第 9 周末 | Memory + 上下文工程 + MCP + 最小评估报告 | 优先补必修与评估，不挤入完整基准或训练 |
| 第 11 周末 | 完成综合项目的改造任务 | 缩小改造范围，保底完成其一 |
| 第 12 周末 | 拥有 1 个可开源的毕业设计 | - |

## 弹性调整建议

- **时间紧张**（每周 <7 小时）：按优先级砍--先砍全部 🟢 P2，再压缩 🟡 P1 的深度（但阶段六综合项目至少保底完成），🔴 P0 一项不动，总周期顺延为 14~16 周
- **已有 Agent 基础**：跳过阶段一，直接从阶段二手敲 ReAct 开始（仍不可跳过）
- **只想做应用**：完整 BFCL/GAIA 与 Agentic-RL 都可后补，但保留第 9 周两天最小评估
- **只求快速入门**（极端情况，4 周方案）：阶段二（2 周）-> 阶段四（1.5 周）-> 8A 的 D1~D3 记忆系统（0.5 周），其余全部舍弃

## 本次更新补充清单

**先区分来源**：本次真正变化是 Coze 教程更新、流式打印修复、视频导航扩充与 16 个社区项目新增。Function Calling、Extra05/08/09/10/11/12 在原课程基线已存在，属于这次补齐原计划覆盖，而非上游刚新增。

| 内容 | 放在哪学 | 完成标准 / 取舍 |
|---|---|---|
| Coze 新界面与新增视频 | 第 4 周低代码二选一 | 完成一个流程即可，不扩张为平台精通路线 |
| 流式打印修复 | 第 5 周基类练习 | 画清 LLM -> 生成器 -> 调用方的输出职责 |
| Function Calling | 第 6 周 D3 | 与文本工具解析做一次受控对比 |
| Skills 写法 + Trace | 第 8 周 D6~D7 | 一份可手动加载的 Skill + 一条可诊断失败轨迹 |
| 第 4/6/8/9/14 章视频 | 按对应阶段查 [Extra13](Extra-Chapter/Extra13-Hello-Agents视频课录制共创.md) | 只在卡点处辅助；“附录二”仍待发布，不排硬性任务 |
| 新增社区案例 | 第 12 周 D1~D2 的原浏览额度 | 选 2~3 个读架构、一个读实现，不要求全量部署 |
| GUI / Web Agent | 核心路线后读 [Extra06](Extra-Chapter/Extra06-GUIAgent科普与实战.md)、[Extra11](Extra-Chapter/Extra11-WebAgent科普与实战.md) | 有浏览器自动化需求再做一个工具，不增加默认账号/付费依赖 |
| 自进化 | 最小评估之后读 [Extra10](Extra-Chapter/Extra10-Agent自进化.md) | 做“失败记录 -> 改一条规则/Skill -> 同集回归 -> 可回滚”闭环，不直接上在线 RL |
| 后训练 | 核心路线后读 [Extra12](Extra-Chapter/Extra12-旅行助手后训练实战.md) | 应用方向先学评测与 Rerank，训练方向再上 GPU |
| HelloAgents 1.0.1 | 第 12 周后另排约 1 周迁移实验 | Python 3.12/3.13 独立环境，只迁移一个小 Demo；对照运行时组件、上下文/持久记忆与工具协议，复跑相同测试集 |

**下一次从这里继续**：你已经勾选完成第 1 周，接下来直接做阶段二 D1，准备第 4 章搜索 Key 并精读 `llm_client.py` / `tools.py`；暂不启动全部社区项目或升级到 1.x。新增资源随章节消化即可。
