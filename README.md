# GreenTeaSkills

通用技能。目录名和 `SKILL.md` 的 `name` 使用 `gt-` 前缀，安装到一起时不会和同名技能冲突。

- `skills/gt-framework-design` — 设计给调用方的框架、库、SDK、DSL、CLI 或共享 API：先定用法契约，按误用后果和失败时机决定校验，缺必需输入就失败关闭，不为违约加补偿。
- `skills/gt-change-hygiene` — 回改未发版改动和 review 意见：先查发布线，源码只留最终设计，规则单点、取值正交。
- `skills/gt-test-isolation` — 写或评审测试时保证隔离性、独立性和局部可读性；不因重复代码牺牲测试隔离。
- `skills/gt-code-shape` — 保持代码形状精简：去掉无语义间接层，按职责拆合文件，只为共享语义抽 helper。
- `skills/gt-constant-extraction` — 判断硬编码值是否该提取为常量，以及放在哪、怎么命名。
- `skills/gt-output-style` — 统一回复写法：先给结论，位置紧跟关键代码片段，不写过程废话。

## 场景路由

| 场景 | Skill |
| --- | --- |
| 设计给调用方的框架、库、SDK、DSL、CLI 或共享 API | `gt-framework-design` |
| 回改未发版改动、回应 review、判断兼容性 | `gt-change-hygiene` |
| 写或评审测试，处理测试隔离与复用 | `gt-test-isolation` |
| 增加/删除 helper、拆分文件、决定注释 | `gt-code-shape` |
| 判断硬编码值是否提取为常量 | `gt-constant-extraction` |
| 所有用户可见回复 | `gt-output-style` |

多个场景可以同时适用。项目专属 skill（例如 `gooq-development`）安装后补进项目自己的 CLAUDE.md 路由。

