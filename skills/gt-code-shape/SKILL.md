---
name: gt-code-shape
description: >-
  How to keep code shape explicit and minimal: remove indirection that adds no
  semantics, keep file names aligned with their responsibilities, and share
  helpers only for a stated contract. MUST use when adding or removing helpers
  or abstractions, splitting or merging files, reviewing code structure, or
  adding, keeping, or removing a comment while writing, editing, or reviewing
  code. Leave constants to gt-constant-extraction.
---

# 怎么保持代码形状

每加一层间接、共享或注释，先说明它增加了什么语义；说不出来就不加。

1. 不保留没有语义的间接层：一层转发、固定错误构造函数、只为少写一行的 helper，直接内联。需要调用方识别错误时用可比较的错误值或错误类型，不要用无法比较的构造函数。
2. 文件名与内容一致，按职责拆合；拆分或合并先做纯移动，再改行为。
3. helper 只在共享同一语义时提取；独立实现的内部细节不为了去重共享。
4. 代码清楚就不写注释；只写非显然约束、顺序、平台/实现差异、安全边界。
5. 常量判断使用 gt-constant-extraction；这里不重复一套规则。
