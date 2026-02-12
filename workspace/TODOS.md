# TODOS

## 约定

- **预期完成日期**：如果没有特别指明，写"无"
- **任务拆分**：如果待办比较复杂，可以用二级子列表将任务拆分，子列表格式与主列表相同
- **完成标准**：任务达到完成标准后与 Nick 进行确认，然后再删除
- **格式**：`(首次记录日期-预期完成日期) 待办事项：描述（包含完成标准）`
- **Random thoughts**：每个待办列表的最后一项下面，默认添加一个 random thoughts 条目，用子列表记录未成形的想法或创意，等想法比较成型后移动到正式待办

---

## Sub-agent 架构设计

- (2026-02-12 - 无) 设计第一个具体的 sub-agent 角色：确定 sub-agent 的类型（Researcher / Coder / Writer / 其他），定义其职责边界和使用场景
- (2026-02-12 - 无) 定义 sub-agent 的价值观和工作规范：编写 sub-agent 专用的 SOUL.md 和 AGENTS.md，确保与主 agent 价值观一致但专注于特定任务
- (2026-02-12 - 无) 测试 sub-agent 的实际效果：创建测试任务，验证 sub-agent 是否按预期工作，评估 token 节省效果
- random thoughts
  - (2026-02-12) 考虑是否需要一个专门处理数据分析的 sub-agent（Data Analyst），可以处理 CSV、JSON 等数据的统计和可视化

## 文件管理优化

- (2026-02-12 - 无) 整理 workspace 文件结构：确保所有文件存放在正确位置，清理根目录下不属于这里的文件
- random thoughts
  - (2026-02-12) 考虑建立一个自动化的 workspace 健康检查脚本，定期扫描并报告文件组织问题

## Assistant improvement

- (2026-02-12 - 无) 寻找并集成有用的 skills：在 OpenClaw skill 仓库和外部资源中搜索 organize、introspect/reflect/learn/improve/evolve 相关 skills，评估其适用性并集成到工作流中
- random thoughts
  - (2026-02-12) 自省时检查工具调用重复错误：在每日自省（02:00）和每周回顾时，系统性地识别工具调用中的重复错误模式，并据此优化相关 skill
  - (2026-02-12) 在技术讨论中更加主动提问，发挥思维伙伴而非记录员的角色
  - (2026-02-12) 养成使用工具验证观点的习惯，将数据融入讨论
