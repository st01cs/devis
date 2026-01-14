# devis

> Interviewing to understand requirements, and then implementing them using a Manus-style approach.

`devis` 是一个 Claude Code 插件，通过结构化的访谈流程理解需求，然后基于 Manus 的上下文工程原则实现功能。它使用持久化的 Markdown 文件作为"磁盘上的工作记忆"，确保在复杂任务中不会丢失目标和进展。

## 核心特性

- **结构化访谈** (`/intv`) - 通过深度访谈明确需求、技术方案和权衡
- **渐进式实现** (`/impl`) - 基于 Manus 风格的工作流程，使用文件系统作为外部记忆
- **上下文工程** - 基于 Manus AI 代理最佳实, 生成 `task_plan.md`、`findings.md`、`progress.md` 三个核心文件
- **Git Worktree 支持** - 在隔离的工作树中安全开发
- **上下文工程** - 基于 Manus 被收购前的生产级 AI 代理最佳实践

## 安装

### 方法 1: Claude Code Marketplace（推荐）

在 Claude Code 中执行：

```bash
claude plugin install st01cs/devis
```

### 方法 2: 手动安装

```bash
# 克隆仓库到 Claude Code 插件目录
git clone https://github.com/st01cs/devis.git ~/.claude/plugins/devis
```

## 使用方法

### 工作流程

devis 采用两阶段工作流：

```
用户需求 → /intv（访谈） → 规划文档 → /impl（实现） → 最终交付
```

### 第一步：访谈规划

使用 `/intv` 命令进行需求访谈：

```bash
/intv path/to/your/plan.md
```

**访谈过程：**

- Claude 会深入询问技术实现、UI/UX、潜在担忧、权衡方案等
- 访谈结束后自动生成三个文件：
  - `task_plan.md` - 阶段划分、进度跟踪、决策记录
  - `findings.md` - 研究发现、访谈内容、技术决策
  - `progress.md` - 会话日志、测试结果

### 第二步：实现

使用 `/impl` 命令按计划实现：

```bash
/impl path/to/your/plan.md
```

**实现过程：**

- 自动在 `.worktrees/` 下创建 Git Worktree
- 按阶段执行计划，每完成一个阶段更新状态
- 记录所有错误和解决方案
- 使用"2-Action 规则"防止丢失多模态信息
- 完成后验证所有阶段完成

## 文件结构

```
devis/
├── .claude-plugin/
│   ├── plugin.json          # 插件元数据
│   └── marketplace.json      # 市场发布配置
├── commands/
│   ├── intv.md              # /intv 命令定义
│   └── impl.md              # /impl 命令定义
├── templates/
│   ├── task_plan.md         # 任务计划模板
│   ├── findings.md          # 研究发现模板
│   └── progress.md          # 进度日志模板
├── refs/
│   ├── manus.md             # Manus 原则参考
│   └── examples.md          # 实战示例
├── scripts/
│   └── check-complete.sh    # 完成度检查脚本
└── README.md
```

## 示例场景

```bash
# 0. 需求草稿
# 在项目目录创建文件 dev-docs/plan/feature-xxx/feature-draft.md简单描述需求

# 1. 访谈需求
/intv dev-docs/plan/feature-xxx/feature-draft.md

# 访谈中会问：设计偏好、状态管理方式、兼容性要求等
# 访谈结束后自动生成三个文件：
# - dev-docs/plan/feature-xxx/task_plan.md
# - dev-docs/plan/feature-xxx/findings.md
# - dev-docs/plan/feature-xxx/progress.md

# 2. 实现功能
/impl dev-docs/plan/feature-xxx/task_plan.md

# 自动创建 worktree、按阶段实现、记录进度
```

## 进阶用法

### 自定义模板

你可以修改 `templates/` 下的文件来定制规划流程：

```bash
# 编辑模板
nano ~/.claude/plugins/devis/templates/task_plan.md
```

## 常见问题

### Q: 为什么需要 Git Worktree？

A: Worktree 提供隔离的开发环境，避免在主分支上直接修改。所有更改在完成后才合并回主分支。

### Q: 如何恢复中断的工作？

A: 直接再次运行 `/impl path/to/task_plan.md`，它会读取现有文件并从当前阶段继续。

## 致谢

本项目的核心理念和方法论深受以下项目启发：

- **Manus** - [Context Engineering for AI Agents](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)

- **planning-with-files** - [planning-with-files](https://github.com/OthmanAdi/planning-with-files)

## 许可证

MIT License - 详见 [LICENSE](LICENSE)

## 作者

st01cs - [GitHub](https://github.com/st01cs)
