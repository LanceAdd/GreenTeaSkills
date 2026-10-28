---
name: gt-code-shape
description: >-
  How to keep code shape explicit and minimal: remove indirection that adds no
  semantics, keep file names aligned with their responsibilities, and share
  helpers only for a stated contract. MUST use when writing, editing, or
  reviewing code that adds, removes, or keeps a helper, splits or merges files,
  or touches a comment — including a comment already next to the edited lines —
  even when the user never mentions structure or comments. Do this: after every
  code edit, review each comment the change touches, added, edited, or still
  explaining the changed lines. Delete it when the code is already clear. Keep
  a comment only for a non-obvious constraint, ordering, platform or
  implementation difference, or security boundary. Before adding a helper,
  wrapper, or file split, state the semantics it adds; if you cannot, inline
  it. Leave constants to gt-constant-extraction.
---

# 怎么保持代码形状

每加一层间接或共享，先说明它增加了什么语义；说不出来就不加。每次改完代码，审查这次改动涉及的注释：代码已经清楚的，删掉。

1. 不保留没有语义的间接层：一层转发、只为少写一行的 helper，直接内联。调用方要识别错误时，用可比较的错误值或错误类型。
2. 文件名与内容一致，按职责拆合；拆分或合并先做纯移动，再改行为。
3. helper 只在共享同一语义时提取；独立实现的内部细节不为了去重共享。
4. 每次改动代码后，审查 diff 涉及的注释：新加的、改过的，以及还在解释这段被改代码的。逐条判断是否必要。代码足够清晰，注释就没必要：不要加，已经在的也删掉。只保留代码看不出来的内容：非显然约束、顺序、平台或实现差异、安全边界。
5. 常量判断使用 gt-constant-extraction；这里不重复一套规则。
6. 收尾再对一遍 diff：该删的注释还在就删掉；一层转发直接内联。

## 例子

- 删：`// 增加计数`、复述函数名、把下一行换种说法再写一遍。代码已经清楚。
- 留：锁必须在读配置之前拿到；这个分支是 Windows 路径分隔符。代码看不出这条约束。
