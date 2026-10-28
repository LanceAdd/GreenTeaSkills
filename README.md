# GreenTeaSkills

通用技能。目录名和 `SKILL.md` 的 `name` 使用 `gt-` 前缀，安装到一起时不会和同名技能冲突。

- `skills/gt-framework-design` — 设计、实现或修改给调用方的框架、库、SDK、DSL、CLI 或共享 API，以及在这些路径上新增或修改默认值、兜底、静默替换、校验分支：先定用法契约，按误用后果和失败时机决定校验，缺必需输入就失败关闭，不为违约加补偿。
- `skills/gt-change-hygiene` — 编辑已有代码、回应 review，或碰到另一层也会解释的值、标记、解析、序列化、校验：先查发布线，改完审查 diff，源码只留最终设计，规则单点、取值正交。
- `skills/gt-test-isolation` — 编写、修改或评审测试时保证隔离性、独立性和局部可读性；不因重复代码牺牲测试隔离。
- `skills/gt-code-shape` — 编写、修改或审阅代码时保持形状精简：去掉无语义间接层，按职责拆合文件，只为共享语义抽 helper。每次改动后审查涉及的注释，代码足够清晰就删掉。
- `skills/gt-constant-extraction` — 编写、修改或审阅时，改动碰到字符串或数字字面量（含改动行上原有的）：先盘点，再判断是否提取为常量，以及放在哪、怎么命名。
- `skills/gt-output-style` — 统一回复写法：先给结论，位置紧跟关键代码片段，不写过程废话。

## 场景路由

| 场景 | Skill |
| --- | --- |
| 设计、实现或修改给调用方的框架、库、SDK、DSL、CLI 或共享 API；在这些路径上新增或修改默认值、兜底、静默替换、校验分支 | `gt-framework-design` |
| 编辑已有代码、回应 review，或碰到另一层也会解释的值、标记、解析、序列化、校验 | `gt-change-hygiene` |
| 编写、修改或评审测试 | `gt-test-isolation` |
| 编写、修改或审阅代码时的 helper、文件拆合；每次改动后审查涉及的注释 | `gt-code-shape` |
| 编写、修改或审阅时改动碰到字符串或数字字面量，含改动行上原有的 | `gt-constant-extraction` |
| 所有用户可见回复 | `gt-output-style` |

多个场景可以同时适用。项目专属 skill 安装后补进项目自己的 CLAUDE.md 路由。

