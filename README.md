# Spec Kit 速查表 | Spec Kit Cheatsheet | Guía rápida de Spec Kit

**先写规格、再让 AI 写代码 —— 规格没过门，代码不许开工**

> 一页看懂 GitHub 官方的 Spec Kit：把「先想清楚要做什么」变成一套可执行的流程。它给 AI 编程助手装上结构化工序 —— 先立项目宪法（constitution），再把需求写成规格（specify），澄清歧义（clarify），出技术方案（plan），拆成任务（tasks），一致性检查（analyze）过了才动手实现（implement），最后拿实现回对规格收敛（converge）。你要管的不再是代码细节，而是规格的质量。

- 上游项目：[github/spec-kit](https://github.com/github/spec-kit)（2026-11-07 实测 148,327 ★ / 13,329 forks，MIT；最新版 v1.3.2，2026-11-06 发布）
- 平台：Python 3.11+ 与 uv；Linux / macOS / Windows；配合受支持的 AI 编程助手使用（Copilot、Claude Code、Codex 等，各家调用语法不同，见命令表注）
- 本页结构：中文主文 → English → Español → Português

---

Apoyo y más: [GitHub Sponsors](https://github.com/sponsors/Ezra-Zhao) · [Gumroad — plantillas y cuadernos que uso](https://ezraflow28.gumroad.com) · [Ko-fi](https://ko-fi.com/ezra777zhao)

## 它是什么、不是什么

| 是 | 不是 |
|---|---|
| 一套给 AI 编程助手用的结构化流程工具包：模板、斜杠命令与可组合的扩展机制（GitHub 官方出品） | 不是又一个编程助手 —— 它不写代码，写代码的是你已在用的 Copilot / Claude Code / Codex，它管的是流程 |
| 规格驱动开发（SDD）的参考实现：spec 先行，plan、tasks、代码都从规格派生 | 不是瀑布文档崇拜：规格是活的 Markdown 文件，随时可改，改完往下重新派生 |
| 有质量门的流水线：clarify / checklist / analyze 三道可选门，专治「在歧义上盖楼」 | 不是装完就自动变好：每一步都要你看过结果再继续，跳过审查等于没装 |
| 1.0 后是可扩展平台：Extension（加命令/流程）、Preset（改模板与标准）、Workflow（自动化）、Bundle（成套分发）、Catalog（组件目录） | 社区 Catalog 里的组件不是官方背书：原仓明说目录条目不等于审计或推荐，装之前先看源码 |

## 安装与初始化卡

前置：Python 3.11+、[uv](https://docs.astral.sh/uv/)、一个受支持的 AI 编程助手。

```bash
uv tool install specify-cli                        # 装 Specify CLI
specify init my-project --integration copilot      # 建项目（integration 换成你的助手）
cd my-project
```

- `--integration` 填助手的 integration key：如 `copilot`（GitHub Copilot）；其他助手查官方 Integrations 页，装错了斜杠命令不会出现在你的助手里。
- 已有项目也能接入（官方有 Existing projects 指南），不必先有空目录。
- 升级走官方 Upgrade 指南；已装的项目用 `specify` 命令查看与管理扩展、预设与工作流（如 `specify extension search`、`specify preset search`、`specify workflow search`、`specify bundle search` 查目录）。

## SDD 九命令速查（核心表）

在助手聊天里逐条运行（不是终端），每条看完结果再下一条。命令书写有三种方言：文档写 `/speckit.xxx`，Copilot 默认技能语法是 `/speckit-xxx`，Codex 等用 `$speckit-xxx`，Kimi 用 `/skill:speckit-xxx` —— 认准你那家的写法。

| 顺序 | 命令 | 产出 | 要点 |
|---|---|---|---|
| 0 | `/speckit-constitution` | 项目宪法（原则） | 每项目只立一次，原则变化再改；后面每一步都拿它当评判标准 |
| 1 | `/speckit-specify` | spec.md（需求与用户故事） | 只写「做什么、为什么」，不写技术栈；这是全流程唯一硬性必经步 |
| 2 | `/speckit-clarify` | 歧义澄清回写进 spec | 可选质量门：最多提 5 个定向问题，可按关注点反复跑；先澄清再规划，别在歧义上设计 |
| 3 | `/speckit-plan` | plan.md（技术方案） | 技术栈、架构、约束在这里给；实现细节的家在这一步，不在 specify |
| 4 | `/speckit-checklist` | 需求质量清单 | 可选质量门：相当于「给需求写单元测试」，查规格完整、清晰、无歧义；勾选是审查者的人工确认，实现不得自批 |
| 5 | `/speckit-tasks` | tasks.md（依赖排序的任务表） | 按 Setup → Foundational → 逐个用户故事 → Polish 分阶段，可并行的任务有标记 |
| 6 | `/speckit-analyze` | 一致性分析报告 | 可选质量门：只读检查 spec/plan/tasks 三件套的冲突与缺口，不改文件；有问题回源头那一步改，再重跑到干净 |
| 7 | `/speckit-implement` | 代码 | 按 tasks 顺序执行；开工前会先读 checklist 勾选状态，未勾完会先问你；大功能分阶段跑，别一次吞完 |
| 8 | `/speckit-converge` | 收敛结论或追加任务 | 只增不改：核对实现与规格的差距，无差距报 `Converged`，有差距把新任务追加进 tasks.md，回去 implement 再 converge，循环到收敛 |

**最短路径**：constitution → specify → plan → tasks → implement → converge，其余三条是按歧义程度加的质量门。赶时间可以跳门，别跳审查。

## 另外两条流程线（扩展）

Spec Kit 1.0 起，SDD 之外还打包了两条独立流程，都是扩展、按需装，与 SDD 无先后依赖：

**修 Bug 线**（终端里先 `specify extension add bug`）：

```text
/speckit-bug-assess "Submitting an empty password crashes the login form." slug=login-crash
/speckit-bug-fix slug=login-crash
/speckit-bug-test slug=login-crash
```

产物在 `.specify/bugs/<slug>/`，最终结论只有 `verified` / `partial` / `failed` 三种，只有 verified 才算修好。

**想法评估线**（终端里先 `specify extension add assess`）：

```text
/speckit-assess-intake "Let users work offline and sync when they reconnect." slug=offline-mode
/speckit-assess-research slug=offline-mode
/speckit-assess-define slug=offline-mode
/speckit-assess-shape slug=offline-mode
/speckit-assess-decide slug=offline-mode
```

产物在 `.specify/assessments/<slug>/`，最终决定是 `go` / `needs-clarification` / `kill`；判 go 的把 decision.md 的交接摘要喂给 `/speckit-specify` 接着做；判 kill 并写明理由，同样是有价值的产出。未知项别整段重跑，直接改对应的 Markdown 产物。

## 同类工具对照（选型参考）

| 工具 | 出身 | 形态 | 与 Spec Kit 的关键差异 |
|---|---|---|---|
| **Spec Kit** | GitHub 官方 | CLI + 模板 + 斜杠命令，助手无关 | 流程最全（SDD＋修 Bug＋想法评估＋扩展生态），148k 星 |
| OpenSpec | Fission-AI（社区） | CLI + 提案/变更文件夹 | 更轻：围绕「变更提案」组织规格，74,215 ★（2026-11-07 实测），上手快、门更少 |
| BMAD-METHOD | bmad-code-org（社区） | 多智能体敏捷框架 | 不止规格：整套 AI 敏捷开发角色与流程，55,099 ★（同日实测），重得多 |
| Kiro | AWS | 独立 IDE | 规格驱动长在自家 IDE 里（specs 功能），换 IDE 成本高；Spec Kit 是贴在你现有助手上 |
| Tessl | Tessl 公司 | 商业产品（spec 注册表方向） | 商业路线，主打「spec 即源码」与注册表；Spec Kit 是开源工具包 |

选型一句话：已在用 Copilot/Claude Code/Codex 且想要官方全套流程，先 Spec Kit；只想给现有项目加一层轻规格，试 OpenSpec；想要整套多智能体敏捷班子，看 BMAD。

## 诚实标注区（这一栏是这张速查的命）

- **流程不是银弹**：Spec Kit 把「需求不清」这个真问题工具化了，但规格写得含糊，九条命令跑完照样产出含糊的代码 —— clarify 与 analyze 抓的是文档之间的一致性，抓不了你脑子里没想清的东西。它的价值在逼你先想，不是替你想。
- **每一步都要人审是设计、不是缺陷**：官方文档反复写明逐条运行、看过再继续。想全自动一键跑完的人会失望；那也不是它承诺的用法。
- **命令方言容易踩坑**：同一条命令在 Copilot 是 `/speckit-specify`、在 Codex 是 `$speckit-specify`、文档里写 `/speckit.specify` —— 照错文档抄是新用户第一大报错来源，先查你那个助手的 invocation 页。
- **社区组件先看源码**：原仓明说 Catalog 条目「不是审计、不是背书」，Extension 会在你的机器上跑代码，装社区扩展/预设之前先读源码，与装任何第三方插件同理。
- **势头数据**：148,327 ★ / 13,329 forks（2026-11-07 原仓实测，证据 A），fork 率约 9%；最新版 v1.3.2（2026-11-06），仍在高频迭代，命令与目录机制后续可能变，引用本页时请同时核对官方文档日期。

---

## English (summary)

Spec Kit (148,327 stars / 13,329 forks as of 2026-11-07, MIT, latest v1.3.2) is GitHub's open-source toolkit that gives AI coding agents a structured process: reusable templates, slash commands, and documented outcomes. Its core process is Spec-Driven Development (SDD): define what to build and why, then carry the specification through planning, implementation, and convergence.

Install with `uv tool install specify-cli`, then `specify init my-project --integration copilot` (Python 3.11+ and uv required). Run the commands one at a time in your agent's chat, reviewing each result: `/speckit-constitution` (project principles, once per project) → `/speckit-specify` (requirements and user stories — the only strictly required step) → `/speckit-clarify` (optional gate; up to five targeted questions that resolve ambiguity before planning) → `/speckit-plan` (technical plan; the tech stack lives here, not in the spec) → `/speckit-checklist` (optional gate; "unit tests for your requirements") → `/speckit-tasks` (dependency-ordered tasks by phase and user story) → `/speckit-analyze` (optional gate; read-only consistency check across spec/plan/tasks) → `/speckit-implement` (executes tasks; reads checklist state as a gate) → `/speckit-converge` (append-only; reports Converged or appends the missing work as new tasks — repeat implement/converge until converged). Command syntax varies by agent: `/speckit-x` in Copilot skills mode, `$speckit-x` in Codex, `/skill:speckit-x` in Kimi.

Two bundled extensions add independent processes: bug fixing (`specify extension add bug`; assess → fix → test, verdict verified/partial/failed) and idea assessment (`specify extension add assess`; intake → research → define → shape → decide, ending in go / needs-clarification / kill). Extensions, presets, workflows, bundles and catalogs make it a platform — but catalog listings are not audits or endorsements, so read community source code before installing. Honest note: the process forces you to think first; it does not think for you. A vague spec still produces vague code, and the per-step human review is by design, not a bug. Alternatives: OpenSpec (lighter, change-proposal based, 74,215 stars), BMAD-METHOD (full multi-agent agile framework, 55,099 stars), AWS Kiro (spec-driven inside its own IDE), Tessl (commercial).

---

## Español (guía rápida — traducción de estudio, pendiente de revisión por un hablante nativo)

**Primero la especificación, después el código: si la especificación no pasa la puerta, el código no empieza.**

Spec Kit (148.327 estrellas / 13.329 forks a 7 de noviembre de 2026, MIT, última versión v1.3.2) es el kit oficial de GitHub que da a los agentes de programación con IA un proceso estructurado: plantillas reutilizables, comandos y resultados documentados. Su proceso central es el desarrollo guiado por especificaciones (SDD).

Instalación: `uv tool install specify-cli` y luego `specify init my-project --integration copilot` (requiere Python 3.11+ y uv). Los comandos se ejecutan uno a uno en el chat del agente, revisando cada resultado: `/speckit-constitution` (principios del proyecto, una vez) → `/speckit-specify` (requisitos e historias de usuario; el único paso estrictamente obligatorio) → `/speckit-clarify` (puerta opcional: hasta cinco preguntas para resolver ambigüedades antes de planificar) → `/speckit-plan` (plan técnico; la pila tecnológica va aquí, no en la especificación) → `/speckit-checklist` (puerta opcional: «pruebas unitarias para tus requisitos») → `/speckit-tasks` (tareas ordenadas por dependencias y por historia de usuario) → `/speckit-analyze` (puerta opcional: análisis de coherencia de solo lectura entre spec/plan/tasks) → `/speckit-implement` (ejecuta las tareas y usa el checklist como puerta) → `/speckit-converge` (solo añade: declara convergencia o agrega el trabajo faltante como tareas nuevas; repite hasta converger). La sintaxis varía según el agente: `/speckit-x` en Copilot, `$speckit-x` en Codex, `/skill:speckit-x` en Kimi.

Dos extensiones añaden procesos independientes: corrección de errores (`specify extension add bug`; evaluar → corregir → probar, veredicto verified/partial/failed) y evaluación de ideas (`specify extension add assess`; intake → research → define → shape → decide, con decisión go / needs-clarification / kill). Los componentes del catálogo comunitario no son auditorías ni avales: lee el código antes de instalar. Nota honesta: el proceso te obliga a pensar primero, pero no piensa por ti; una especificación vaga sigue produciendo código vago, y la revisión humana paso a paso es parte del diseño. Alternativas: OpenSpec (más ligero, 74.215 estrellas), BMAD-METHOD (marco ágil multiagente completo, 55.099 estrellas), Kiro de AWS (guiado por especificaciones dentro de su propio IDE) y Tessl (comercial).

---

## Português (guia rápido — tradução de estudo, aguardando revisão de um falante nativo)

**Primeiro a especificação, depois o código: se a especificação não passar pela porta, o código não começa.**

O Spec Kit (148.327 estrelas / 13.329 forks em 7 de novembro de 2026, MIT, versão mais recente v1.3.2) é o kit oficial do GitHub que dá aos agentes de programação com IA um processo estruturado: modelos reutilizáveis, comandos e resultados documentados. Seu processo central é o desenvolvimento orientado por especificações (SDD).

Instalação: `uv tool install specify-cli` e depois `specify init my-project --integration copilot` (requer Python 3.11+ e uv). Os comandos rodam um de cada vez no chat do agente, revisando cada resultado: `/speckit-constitution` (princípios do projeto, uma única vez) → `/speckit-specify` (requisitos e histórias de usuário; o único passo estritamente obrigatório) → `/speckit-clarify` (porta opcional: até cinco perguntas para resolver ambiguidades antes de planejar) → `/speckit-plan` (plano técnico; a pilha tecnológica mora aqui, não na especificação) → `/speckit-checklist` (porta opcional: «testes unitários para os seus requisitos») → `/speckit-tasks` (tarefas ordenadas por dependência e por história de usuário) → `/speckit-analyze` (porta opcional: análise de consistência somente leitura entre spec/plan/tasks) → `/speckit-implement` (executa as tarefas e usa o checklist como porta) → `/speckit-converge` (só acrescenta: declara convergência ou adiciona o trabalho que falta como novas tarefas; repita até convergir). A sintaxe varia por agente: `/speckit-x` no Copilot, `$speckit-x` no Codex, `/skill:speckit-x` no Kimi.

Duas extensões adicionam processos independentes: correção de bugs (`specify extension add bug`; avaliar → corrigir → testar, veredito verified/partial/failed) e avaliação de ideias (`specify extension add assess`; intake → research → define → shape → decide, com decisão go / needs-clarification / kill). Os componentes do catálogo da comunidade não são auditorias nem endossos: leia o código antes de instalar. Nota honesta: o processo obriga você a pensar primeiro, mas não pensa por você; uma especificação vaga continua gerando código vago, e a revisão humana passo a passo faz parte do desenho. Alternativas: OpenSpec (mais leve, 74.215 estrelas), BMAD-METHOD (framework ágil multiagente completo, 55.099 estrelas), Kiro da AWS (orientado por especificações dentro do próprio IDE) e Tessl (comercial).

---

## 来源与声明

- 灵感来源 / Source / Fuente / Fonte: [github/spec-kit](https://github.com/github/spec-kit)（文档站 github.github.io/spec-kit；最新版 v1.3.2，2026-11-06）
- 本仓是个人学习整理：内容用我自己的话总结自上游公开文档，不搬运原文；命令清单与流程顺序以 2026-11-07 的上游 README 与官方 Agentic SDD 命令参考页为准，对照仓星数以同日 GitHub 实测为准，上游迭代后请以官方文档为最终依据。
- 西语、葡语部分为学习译稿，待母语校对。Las secciones en español y portugués son traducciones de estudio pendientes de revisión nativa.
- License: MIT © 2026 Guangyi Zhao
