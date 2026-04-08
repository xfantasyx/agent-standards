# Spec 规格层（Implementation Spec）

在设计评审和编码之间增加一个 **Spec 层**——将设计方案转化为机械可验证的编码契约。

## 核心理念

设计方案是人类可读的意图描述，Spec 是机器可验证的编码契约。Agent 按 Spec 逐条实现，评审时逐条对照。

```
设计方案（人类意图）
    │
    ▼ /spec-gen
┌─────────────────────────────────────┐
│  Spec 文件（编码契约）               │
│  ── 精确的接口签名                   │
│  ── MUST / MUST NOT 边界约束         │
│  ── 验收断言（可映射到测试用例）      │
│  ── source_hash 追踪设计变更         │
└──────────────┬──────────────────────┘
               │ /spec-code
               ▼
           编码实现
```

## 触发条件

以下任务必须先生成 Spec 再编码：

- 定义对外接口的模块
- 涉及跨模块依赖（有 imports）的任务
- 算法实现类任务
- 简单工具类/配置类任务可跳过

## Spec 文件格式

存放于项目 `specs/` 目录下，每个任务一个文件：

```markdown
---
task: T-008                              # 对应 TASKS.md 的任务 ID
title: "高度图生成器"
depends: [T-007, T-006]                  # 前置依赖
design_source: 开发规划.md#day-2-3       # 设计来源
source_hash: "fd91b244294a70db"          # 设计源内容 hash
output_files:                            # 必须产出的文件
  - src/heightmap/heightmap_generator.h
  - src/heightmap/heightmap_generator.cpp
exports:                                 # 本任务导出的接口
  - "HeightmapGenerator"
  - "HeightmapConfig"
imports:                                 # 消费的上游接口
  - "fbm()"                             # from T-007
  - "voronoi2d()"                        # from T-006
---

# 高度图生成器

## 概述
（2-3 句描述实现目标）

## 依赖类型（内联）
（从上游 Spec 提取的类型声明，标注真源）

## 接口定义
（完整的接口声明，可直接复制到源文件）

## 边界约束
- MUST: 支持 256x256 到 8192x8192 分辨率
- MUST: 输出值范围 [0.0, 1.0]，不出现 NaN/Inf
- MUST: 相同 seed + config 产生相同结果（确定性）
- MUST NOT: 在生成过程中分配堆内存超过输出 buffer 的 2 倍
- MUST: 支持多噪声层叠加，层数 ≥ 1

## 实现要求
（算法细节、性能要求、线程安全等）

## 文件结构
（目录布局 + 各文件职责）

## 验收断言
- ASSERT: 1024x1024 生成结果值域在 [0.0, 1.0] 内
- ASSERT: seed=42 两次生成结果逐像素相同
- ASSERT: 分辨率 256x256 生成耗时 < 100ms
- ASSERT: 8 层 FBM 叠加无数值溢出
- ASSERT: voronoi 权重为 0 时退化为纯 FBM
```

## source_hash 机制

追踪 Spec 与设计源文档之间的新鲜度：

```
计算方法：
1. 读取 design_source 指定的文件（或章节）
2. 去除空行，trim 每行
3. SHA-256 取前 16 位 hex
```

**用途**：
- 设计文档修改后，自动检测哪些 Spec 的 source_hash 不再匹配
- 不匹配的 Spec 标记为 `stale`，必须更新后才能继续编码
- 防止设计改了但代码还是按旧设计写的

## exports / imports 一致性验证

| 检查项 | 说明 |
|--------|------|
| **imports 可追溯** | 每个 imports 条目必须能追溯到依赖链中某个任务的 exports |
| **签名一致** | imports 的类型签名必须与上游 exports 逐字匹配 |
| **依赖完整** | 使用了上游接口但未声明 imports → 报错 |
| **无孤立 exports** | exports 未被任何下游 imports 引用 → 警告（可能是叶子模块） |

## Spec 生成流程

```
Step 1: 选择任务（status 为 planned 或 spec_ready）
Step 2: 读取设计源文档，计算 source_hash
Step 3: 解析依赖链，提取上游 exports 类型声明
Step 4: 编写 Spec（接口定义 + 边界约束 + 验收断言）
Step 5: 自审查（类型一致性 + 接口签名匹配上游）
Step 6: 人工确认 Spec
Step 7: 更新任务状态 → spec_ready
```

## 任务状态扩展

引入 Spec 层后，任务状态从 3 态扩展为 4 态：

```
🔴 planned ──Spec生成──→ 📋 spec_ready ──编码──→ 🟡 in_progress ──评审通过──→ 🔵 done
```

| 状态 | 含义 | 允许的操作 |
|------|------|-----------|
| 🔴 planned | 已规划，尚未生成 Spec | 生成 Spec |
| 📋 spec_ready | Spec 已就绪，可开始编码 | 开始编码 |
| 🟡 in_progress | 编码进行中 | 编码、评审 |
| 🔵 done | 评审通过 + 人工验收 | 归档 |
