---
name: gt-code-shape
description: >-
  How to keep code shape explicit and minimal: remove indirection that adds no
  semantics, keep file names aligned with their responsibilities, and share
  helpers only for a stated contract. MUST use when writing, editing, or
  reviewing code that adds, removes, or keeps a helper, splits or merges files,
  or adds, keeps, or removes a comment, even when the user never mentions
  structure or comments. Do this: before adding a helper, wrapper, file split,
  or comment, state the semantics it adds; if you cannot, inline it. Write a
  comment only for a non-obvious constraint, ordering, platform or
  implementation difference, or security boundary. Leave constants to
  gt-constant-extraction.
---

# 怎么保持代码形状

每加一层间接、共享或注释，先说明它增加了什么语义；说不出来就不加。

1. 不保留没有语义的间接层：一层转发、只为少写一行的 helper，直接内联。调用方要识别错误时，用可比较的错误值或错误类型。
2. 文件名与内容一致，按职责拆合；拆分或合并先做纯移动，再改行为。
3. helper 只在共享同一语义时提取；独立实现的内部细节不为了去重共享。
4. 代码清楚就不写注释；只写非显然约束、顺序、平台/实现差异、安全边界。
5. 常量判断使用 gt-constant-extraction；这里不重复一套规则。
6. 收尾看 diff：删掉只复述代码的注释；一层转发直接内联。

## 例子

- 不写：`// 增加计数`、复述函数名。
- 写：锁必须在读配置之前拿到；这个分支是 Windows 路径分隔符。
