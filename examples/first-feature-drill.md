# 第一次用 Spec Kit 演练 | First Spec Kit drill: one small feature, full pipeline

把九条命令在一个小功能上完整走一遍，顺序别乱（先小功能、每步看过再继续）：

1. **装与初始化**：确认 Python 3.11+ 与 uv 就绪，`uv tool install specify-cli`，然后 `specify init drill-app --integration copilot`（integration 换成你自己的助手），进目录，启动你的编程助手。
2. **立宪法**：在助手聊天里跑 `/speckit-constitution Create principles focused on code quality, testing, and maintainability.` —— 看一眼生成的宪法，原则不合意就当场改，这是后面所有步骤的评判标准。
3. **写一个小规格**：挑一个一小时能做完的小功能（比如「给本地笔记应用加一个按日期分组的列表页」），跑 `/speckit-specify` 并把功能说具体。检查产出的 spec.md：只该有「做什么、为什么」，出现技术栈就是串味了，删回 plan 步再说。
4. **过两道门**：跑 `/speckit-clarify` 回答它提的问题（最多 5 个），再跑 `/speckit-checklist` 看需求质量清单。清单勾选是你的审查结论，别让助手自己勾。
5. **出方案与任务**：`/speckit-plan` 给出你的技术栈与约束，`/speckit-tasks` 生成任务表后跑 `/speckit-analyze` —— 有冲突先回源头（spec / plan / tasks 各归各步）改干净，再往下走。
6. **实现与收敛**：`/speckit-implement` 执行，跑起来验一遍；再 `/speckit-converge`，若它追加了任务，implement 一轮再 converge，直到它报 Converged。这时回头数一遍：哪一步抓出的毛病最多？那一步就是你以后不许跳的门。
