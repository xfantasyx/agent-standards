# Agent 编码标准（Agent Coding Standards）

通用的 AI Agent 编码质量管控体系，适用于所有项目。

## 文件清单

| 文件 | 说明 |
|------|------|
| [design-review.md](design-review.md) | 编码前设计评审流程与模板 |
| [spec-layer.md](spec-layer.md) | Spec 规格层（编码契约，接口签名+边界约束+验收断言） |
| [test-first.md](test-first.md) | 测试先行（TDD）规范 |
| [self-check-gates.md](self-check-gates.md) | 自检门（编码阶段前置检查 H/I/G） |
| [incremental-checkpoint.md](incremental-checkpoint.md) | 增量检查点机制 |
| [quality-gate.md](quality-gate.md) | 13 项质量评审门禁细则 |
| [never-again.md](never-again.md) | Never Again 规则引擎机制 |
| [patterns.md](patterns.md) | 模式库规范（"应该怎么做"） |
| [regression.md](regression.md) | 回归验证规则 |
| [performance-baseline.md](performance-baseline.md) | 性能基线规范 |
| [automated-verification.md](automated-verification.md) | 自动化验证脚本体系 |
| [cross-agent-review.md](cross-agent-review.md) | 交叉评审机制（编码/评审分离） |
| [anti-bias-review.md](anti-bias-review.md) | 对抗自评偏差（举证式评审） |
| [circuit-breaker.md](circuit-breaker.md) | 评审熔断机制（防无限循环） |
| [human-review-tiers.md](human-review-tiers.md) | 人工审核分级（L1/L2/L3） |
| [visual-review.md](visual-review.md) | 视觉质量评审协议 |
| [safeguards.md](safeguards.md) | 安全保障（hash雪崩/规则质量/并行冲突/测试分级/冷启动/回滚） |
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
- `specs/` — Spec 文件目录（编码契约）
- `rules/` — Never Again 规则库（项目特有）
- `benchmarks/` — 性能基线数据（项目特有）
- `patterns.md` — 项目专属模式库（编码模式与设计决策）
- `tools/` — 自动化验证脚本（逐步建设）
