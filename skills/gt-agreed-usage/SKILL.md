---
name: gt-agreed-usage
description: >-
  How to design a feature so callers follow a stated usage and the implementation stays
  on that path. MUST use when designing a feature or API, adding a callback that replaces
  built-in behavior, or about to add validation, fallback, silent repair, or a second
  result for misuse. Do this: write the required usage as the whole spec, implement only
  that path, and do not add a branch whose job is to compensate for callers who ignore
  the usage. An explicit opt-out such as nil or a documented empty value is part of the
  usage, not a repair. A property you cannot see from one return value stays in the usage
  text. This is an agent procedure, not a tutorial to explain to the user.
---

# 怎么只实现约定用法

功能先规定用户必须怎么做，实现只覆盖这一种用法。不为不按要求用的人再写一套行为。

1. 写出约定用法。它就是这个功能的全部规格。约定里没有的行为，不进实现。
2. 只实现这条路径。用户守约时，结果能从主路径直接看出来。
3. 准备加判断、兜底、静默替换或失败分支时，先问它属于约定用法，还是在给违约安排结果。后者不做。
4. 过程性质看一次返回值证明不了，就留在用法说明里。例如每次调用都要产生新值、要有足够熵、要保持顺序。
5. 显式放弃是用法的一部分。传 `nil`，或文档写明的空值，表示仍用原有行为。这不是对一个不合格结果的补救。
6. 违约可以得出简陋甚至错误的结果。不要再发明一种「正确」把违约掩盖掉。

## 判断一个分支该不该写

- 该写：没有它，约定用法本身就不成立。例如约定「空字符串表示继续用内置实现」，那就只在空字符串时走内置实现。
- 不写：它根据返回值猜测用户失约，然后改写成框架觉得对的结果。一次返回值看不出下一次会不会变，也看不出有没有随机性。
- 不写：它让同一次调用里出现两套策略。读代码的人无法知道跑的是用户的用法，还是框架的补偿。
