---
name: gt-framework-design
description: >-
  How to design caller-facing frameworks, libraries, SDKs, DSLs, CLIs, and
  shared APIs: state the usage contract, validate based on misuse consequence
  and failure timing, fail closed on missing required inputs, and never
  compensate for misuse. MUST use when designing, implementing, or changing a
  caller-facing framework, library, SDK, DSL, CLI, or shared API, and when
  adding or changing a default, fallback, silent replacement, or validation
  branch on that path, even when the user never mentions framework design,
  validation, or fallback. After the edit, review those branches in the diff
  and keep one only when the usage contract requires it. An explicit opt-out
  is part of the usage, not a repair. Does not apply to internal application
  policy unless it is itself a caller-facing contract. Validate when misuse
  would silently corrupt data or fail after a side effect; fail closed on
  missing required inputs.
---

# 怎么设计给别人用的框架或工具

适用于需要向调用方承诺行为的边界：框架、库、SDK、DSL、CLI、shared API。不适用于纯应用内部业务策略或产品级兜底；除非它本身也是对外契约。

1. 先写出约定用法。它就是这个功能的全部规格；约定里没有的行为，不进实现。
2. 只实现这条路径。用户守约时，结果能从主路径直接看出来。
3. 准备加判断、兜底、静默替换或失败分支时，先问：它属于约定用法，还是在给违约安排结果？后者不做。
4. 过程性质看一次返回值证明不了，就留在用法说明里：每次调用产生新值、足够熵、保持顺序等。
5. 显式放弃是用法的一部分。传 nil 或文档写明的空值表示仍用原有行为；这不是对不合格结果的补救。
6. 违约可以得出简陋甚至错误的结果。不要再发明一种“正确”把违约掩盖掉。静默损坏或副作用之后才失败的，按下一节校验；缺必需输入就失败关闭。
7. 改完审查 diff 里的默认值、兜底、静默替换和校验分支。约定用法不需要的，删掉。

## 什么时候加校验或兜底

先判断误用的后果和归属：

- 静默产生错误结果或数据损坏 → 默认值得代码强制。
- 立刻、明显、可诊断地失败 → 默认不重复校验；如果失败发生在副作用之后，或错误难以诊断，可以提前检查。
- 属于调用方的业务策略 → 写进约定和文档，不替调用方决定。
- 下层（编译器、平台、存储）会拒绝 → 默认不重复；同样只在失败晚于副作用或难以诊断时提前检查。

只是少写一次调用或“感觉更安全”的检查不加。缺失必需输入时失败关闭，不猜默认值。

## 判断一个分支该不该写

- 该写：没有它，约定用法本身就不成立。例如约定“空字符串表示继续用内置实现”，那就只在空字符串时走内置实现。
- 不写：它根据返回值猜测用户失约，然后改写成框架觉得对的结果。一次返回值看不出下一次会不会变，也看不出有没有随机性。
- 不写：它让同一次调用里出现两套策略。读代码的人无法知道跑的是用户的用法，还是框架的补偿。
