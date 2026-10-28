---
name: gt-change-hygiene
description: >-
  How to iterate on a change and respond to review feedback without carrying history or
  drift. MUST use when editing existing code, answering review comments, or touching a
  value, tag, parser, serializer, or validation that another layer also interprets,
  even when the user never mentions compatibility or history. Do this: before writing
  compatibility code or a special-case rejection, check whether the old behavior was
  ever released; after the edit, review the diff and delete unreleased intermediate
  branches, flags, tests, and historical comments; keep only the final design; define
  each cross-layer rule once; keep each tag value semantically orthogonal; verify a
  review's premises before accepting its conclusions. This is an agent procedure, not
  a tutorial to explain to the user.
---

# 怎么回改一个改动

先判定这个改动对外存在过没有，再决定要不要兼容、要不要留痕。最终源码只呈现最终设计。

1. 查发布线：这个行为进过 release 吗？从哪个版本开始？谁可能依赖？
2. 没发过的中间形态不是兼容基线。源码只留最终设计；被最终设计取代的分支、标记和测试删掉。不写迁移层，不为中间形态加特殊拒绝，不在注释或测试里写「旧值」「旧行为」。
3. 没发过，就把旧标记还原成普通输入：它只是不再有特殊含义，和其他未识别的值一样处理，不单独开分支。
4. 发过版，才需要兼容：写清楚影响窗口、受影响用户和回退方式，兼容代码本身也要有测试。
5. 同一条规则只定义一次：同一输入被两层以上解释时（校验/文档生成、客户端/服务端、解析/序列化），用命名常量加共享校验收口，两边都测。默认会漂移，对齐是工程动作。
6. 每个值只表达一个维度：位置、来源、形状、格式分开表达，形状尽量交给类型系统。加新值前问两句——是否重复了已有信息？是否会和未来语法（例如用户自定义 key）撞名？
7. Review 意见按「前提 → 结论 → 动作」拆：先验证评论依赖的事实（版本时间线、实际行为、受影响用户群），再判断结论，再决定改不改。接受对的，拒绝错的并给出证据；不因为来自维护者或 bot 就全盘接受。
8. 收尾审 diff：代码、注释、测试里不能有只服务于历史的痕迹，历史属于 PR 描述和 commit message。本次自己引入但最终没人调用的 API、抽象、测试，直接删。

## 判断

- 想问「要不要加兼容或特例」时：先回答「旧行为发过版吗」。没发过，答案就是不要。
- 想写「old / legacy / 以前」的注释或测试时：删掉，改成对最终行为的描述。
- 发现同一条规则在两处判断：合并成一个定义，两边引用同一常量或函数。
- 新值能由字段类型推导出来：不要写进值里。既不重复信息，也不会和未来语法撞名。
- 不确定评论对不对时：先查发布记录和旧版本行为，再回复。核实成本远低于写一层错误的兼容代码。

## 不要做

- 因为「怕有人已经用了」就给未发布行为加迁移代码或特殊拒绝。
- 在源码里保留被本 PR 覆盖掉的中间态说明或测试。
- 让校验和文档生成各自维护一套取值规则。
- 把数据类型（数组、对象、原始字节）写死进取值枚举，除非它确实是唯一要扩展的维度。
- 全盘接受 review：不核实版本时间线就写兼容层。
- 留下本次新增但最终没有调用方的公开 API。
