# Agent 编码标准（Agent Coding Standards）

通用的 AI Agent 编码质量管控体系，适用于所有项目。

## 文件清单

| 文件 | 说明 |
|------|------|
| [design-review.md](design-review.md) | 编码前设计评审流程与模板 |
| [test-first.md](test-first.md) | 测试先行（TDD）规范 |
| [incremental-checkpoint.md](incremental-checkpoint.md) | 增量检查点机制 |
| [quality-gate.md](quality-gate.md) | 12 项质量评审门禁细则 |
| [never-again.md](never-again.md) | Never Again 规则引擎机制 |
| [patterns.md](patterns.md) | 模式库规范（"应该怎么做"） |
| [regression.md](regression.md) | 回归验证规则 |
| [performance-baseline.md](performance-baseline.md) | 性能基线规范 |
| [review-report-template.md](review-report-template.md) | 评审报告模板 |
| [full-workflow.md](full-workflow.md) | 完整评审流程总览 |

## 使用方式

在项目的 `CLAUDE.md` 或开发规划中引用：

```markdown
Agent 编码标准参见：~/.claude/agent-standards/
项目专属规则库：./rules/
项目性能基线：./benchmarks/
```

## 项目层需自建

每个项目需要自建以下文件/目录：
- `rules/` — Never Again 规则库（项目特有）
- `benchmarks/` — 性能基线数据（项目特有）
- `patterns.md` — 项目专属模式库（编码模式与设计决策）
