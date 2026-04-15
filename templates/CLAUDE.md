# CLAUDE.md — ProjectTemplate

Behavioral guidelines to reduce common LLM coding mistakes.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

## Quality Standards

Follow the Agent coding quality system.
Repo: https://github.com/fantasy/agent-standards

Default task tier: **L3** (complex tasks — full workflow applies)

Active specs:
- `full-workflow.md` — end-to-end lifecycle, phase gates, failure handling
- `spec-and-design.md` — design contract required before coding
- `test-first.md` — write tests before implementation
- `coding-and-checkpoints.md` — checkpoint every 30 min, H/I/G self-check gates
- `quality-gate.md` — 13-item graded review before any merge
- `cross-agent-review.md` — separate reviewer agent for L3 tasks
- `safeguards.md` — rollback protocol, hash avalanche detection
- `never-again.md` — load before touching high-risk modules

---

## This Project

**Name:** ProjectTemplate
**Language:** C++ (C++17)
**Build system:** CMake

### Commands

```bash
# Configure
cmake -B build -DCMAKE_BUILD_TYPE=Debug

# Build
cmake --build build --parallel

# Test
ctest --test-dir build --output-on-failure

# Clean
cmake --build build --target clean
```

### Key conventions
- Headers in `include/`, sources in `src/`, tests in `tests/`
- One class per file, filename matches class name
- Use `nullptr` not `NULL`
- RAII for all resource management — no raw `new`/`delete`
- Prefer `std::` algorithms over hand-rolled loops

### Never do
- No `using namespace std` in headers
- No raw owning pointers — use `std::unique_ptr` / `std::shared_ptr`
- No `#pragma once` mixed with include guards — pick one per project
- No commits with failing tests
