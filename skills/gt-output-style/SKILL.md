---
name: gt-output-style
description: >-
  How to write every user-visible reply. MUST follow before answering, explaining code,
  reviewing, comparing options, or reporting progress, even when the user never mentions
  style or output. Do this: state the conclusion in one or two sentences; pair every
  path/file.go:123 with the key code snippet immediately after it — a line number alone
  is not evidence; use a diff or side-by-side snippet for comparisons. If there is
  no code to cite, give the conclusion and say what was not verified. Do not invent
  a snippet. This is an agent procedure, not a tutorial to explain to the user.
---

# 怎么回复

用户在本次对话里明确要求的格式优先；其余可见回复按本技能写。

先做这三步，再写正文。

1. 用 1～2 句写出结论。
2. 每个结论配一组证据：`path/file.go:123`，下一行就是关键代码片段。只有行号、没有片段，不算证据。
3. 下一步只在需要用户决定或继续动手时写。

简单问题按这三步直接写，不加标题。复杂问题再分点，每点仍是「结论，然后位置加片段」。

## 贴代码

- 先写 `path/file.go:123`，紧跟着贴片段。
- 片段只留能证明该结论的那几行，不贴整文件，也不用文字转述代码。
- 一处结论对应一处片段。多个位置就多组，不要只列路径。
- 没读到代码、没验证就直说。不要用「应该」「可能」掩盖，也不要编片段。

## 做对比

- 先给判断：选 A、选 B，或都不选，一句话说原因。
- 能给 diff 就给 diff；能给并排片段就给并排片段。
- 表格只放关键差异，不要把整段代码塞进表格。

```go
// before
fdMap["https"]

// after
fdMap[schemeHttps]
```

## 用词

- 短句，少形容词。
- 不重复用户的问题，不写「让我来」「综上所述」。
- 不用 emoji，除非用户要求。
- 中文对话用中文。标识符、命令、路径用 inline code。
- 不为了格式而加标题、表格、列表。

## 写评审

- 先给判断：对、不对、推荐、不推荐，一句话理由。
- 再给 `文件:行号` 和对应的关键片段。
- 指出问题时给最小修改，能贴修改后的代码就贴。

## 写进度

- 按顺序写：做了什么、改在哪（位置加片段）、验证结果、下一步。
- 不逐条复述过程，不贴大段日志。
