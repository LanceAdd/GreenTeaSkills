---
name: gt-constant-extraction
description: >-
  How to handle hardcoded strings and numbers while writing or reviewing code, then where to
  put a constant and what to name it. MUST use when writing, editing, or reviewing code that
  touches a string or number literal, including one already on the edited lines, even when
  the user never mentions magic values, constants, or duplicate strings. Do this: after
  every code edit, inventory the literals the change touches. Skip literals in tests,
  imports, comments, and generated code. Extract only when at least two of these are true —
  multiple sites must stay identical, a typo would hide a bug, the literal hides its
  meaning — and the sites must not diverge later. Prefer an existing constant. Do not
  extract one-off logs, error text, or standard values such as HTTP methods and date
  layouts. This is an agent procedure, not a tutorial to explain to the user.
---

# 怎么处理硬编码值

按这个顺序做。常量是为契约和含义付费，不是为重复付费。

1. 盘点本次改动碰到的字符串和数字，包括改动行上原来就有的。排除测试、导入、注释、生成代码。
2. 对每个值做下面的判定。不通过就保持字面量。
3. 通过后先找已有常量。能复用就复用，不新造名字。
4. 没有可复用的，再按「放哪里、怎么命名」新增。默认只做高价值。
5. 改完做内联测试：把常量改回字面量，代码没有明显变差就撤回。
6. 跑格式化、静态检查和相关测试。审 diff，去掉只为对齐产生的噪声。

## 判定

前三项至少两项为「是」，并且隔离性是「必须一致」，才提取。

1. 一致性：两个以上位置是否必须永远保持同一个值？例如生产/消费、写入/读取、协议两端。
2. 易错性：空格、大小写、编码、顺序写错，是否会产生隐蔽 bug？
3. 表达性：字面量是否看不出含义，或者它是公开、用户可见的词汇？
4. 隔离性：这个值必须永远一致，还是不同调用点以后可能各自调整？
   - 必须一致，可以共享。
   - 可能分化，例如文案、错误提示、上下文相关的句子，不共享。

重复、跨文件、跨表只是线索。内联测试才是最后裁决：改回字面量后代码明显变差，保留常量；否则保持字面量。

## 价值

- 高：真实契约且有漂移风险；字节敏感；公开词汇；策略魔法数；已有常量可复用。直接做。
- 中：单处重复且命名有明确收益；跨文件但自解释、风险低。做内联测试，改回字面量后代码明显变差才提取。
- 低：单次使用；同处一两次；标准惯用值；展示、日志、错误文案；标签或注解；局部哨兵；相邻表里的稳定重复。不动。
- 不做：把简单查找表做成枚举或注册表；为了「更准确」重命名已有标识符。

## 放置和命名

- 常量放在拥有该契约的包。跨文件放在包内共享位置，单文件就近放。
- 不建全局常量包。优先复用已有常量和标准库，不造一套平行名字。
- 默认私有。只有调用方需要传入或比较时才导出。
- 命名跟所在包走：大小写、前缀、缩写与已有常量家族一致。
- 名字写角色或含义，不写位置。用完整单词，不用像拼错的截断：`levelCodeDebug`，不要 `levelCodeDebu`。
- 能看懂就尽量短。不用 `value`、`key`、`str` 这类没有信息的名字。
- 类型已经说明是字符串时，不要再叫 `stringValue`。
- 已有常量能覆盖就原样复用，不为了更准确改私有名字。
- 注释只写代码看不出来的约束、原因或坑。代码已经清楚就不写。

## 不要做

- 单次使用也提取。
- 为标准写法提取，例如日期格式、HTTP 方法、操作系统名称。
- 把以后可能各自修改的文案合成一个常量。
- 为语义精确改已有私有名字。
- 让查找表混用字面量和标识符。
- 为简单查找表造枚举或注册表。
- 一个常量引起整块格式化对齐。先怀疑这个常量值不值得提。

## 例子

- 值得：协议两端的消息名、带空格的 SQL 片段、用户会传入的公开词汇。
- 不值得：一次性的响应头值、标准日期布局、日志或错误文案。
