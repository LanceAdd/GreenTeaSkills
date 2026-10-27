---
name: gt-test-isolation
description: >-
  How to keep tests isolated and readable: each test owns its state, scenario
  setup and assertions stay local, and shared helpers are limited to environment
  contracts and stateless mechanics. MUST use when writing, editing, refactoring,
  or reviewing tests, even when the user never mentions isolation, fixtures, or
  helpers. Do this: keep arrange/act/assert visible; never share mutable
  fixtures, global state, or a common test harness across modules; duplicate
  scenario code rather than coupling tests.
---

# 怎么保证测试隔离和可读性

测试首先是可独立运行、可独立理解的规格；复用是次要目标。

1. 一个测试聚焦一个行为，arrange / act / assert 在测试体内可见；单独跑、乱序跑、并行跑都成立。
2. 每个测试自己拥有状态：数据、配置、临时资源、连接、替身预期。用唯一数据、框架清理机制或回滚；不依赖执行顺序或共享可变 fixture。
3. 场景数据、场景断言默认留在测试里。重复不是提取理由；不同 setup、不同断言的用例不要为了减行合并成表格测试。同一 setup、只变一个输入轴、断言形状相同的用例，可以做成表格测试。
4. 只共享环境初始化和无状态工具：外部资源准备、临时目录、公共配置、清理、通用格式化。不要共享场景 setup、可变 fixture、全局状态、依赖顺序的 helper，或跨模块的统一测试脚手架。
5. 提取前确认：不是被测行为；所有调用点会一起改；helper 不持有可变状态；抽后仍能直接看懂 arrange / act / assert。任一条不满足，就保留本地重复。
6. 收尾检查：单独/乱序/并行运行；改一个 helper 是否牵连大量无关测试；失败是否仍直接指向当前行为和期望值。
