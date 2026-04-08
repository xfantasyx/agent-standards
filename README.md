# Agent 编码标准（Agent Coding Standards）

通用的 AI Agent 编码质量管控体系，适用于所有项目。

## 文件清单（11 个）

| 文件 | 说明 |
|------|------|
| [full-workflow.md](full-workflow.md) | 完整编码流程总览（端到端） |
| [spec-and-design.md](spec-and-design.md) | 设计评审 + Spec 规格层（编码契约） |
| [test-first.md](test-first.md) | 测试先行（TDD）规范 |
| [coding-and-checkpoints.md](coding-and-checkpoints.md) | 增量检查点 + 自检门（编码阶段） |
| [quality-gate.md](quality-gate.md) | 13 项分级评审 + 举证式打分 + 熔断 + 回归 + 性能基线 + 报告模板 |
| [cross-agent-review.md](cross-agent-review.md) | 交叉评审机制（L3 必须，L1/L2 可选） |
| [human-review-tiers.md](human-review-tiers.md) | L1/L2/L3 人工审核分级 + 视觉任务评审 |
| [never-again.md](never-again.md) | Never Again 规则引擎（上限100条，自动过期，按需加载） |
| [automated-verification.md](automated-verification.md) | 自动化验证脚本体系 |
| [safeguards.md](safeguards.md) | 安全保障（hash雪崩/并行冲突/回滚机制） |
| [README.md](README.md) | 本文件（索引） |

## 使用方式

在项目的 `CLAUDE.md` 中引用：

```markdown
Agent 编码标准参见：~/agent-standards/
```

## 项目层需自建

每个项目需要自建以下文件/目录：
- `specs/` — Spec 文件目录（编码契约）
- `rules/` — Never Again 规则库（项目特有）
- `benchmarks/` — 性能基线数据（项目特有）
- `patterns.md` — 项目专属模式库（编码模式与设计决策）
- `tools/` — 自动化验证脚本（逐步建设）
