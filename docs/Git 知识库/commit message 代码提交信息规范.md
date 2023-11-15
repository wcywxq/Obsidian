---
title: commit message 代码提交信息规范
url: https://www.yuque.com/wcywxq/uv8hkw/th227e
---

推荐的统一格式为：`<type>: <subject>` <a name="CAmyQ"></a>

## type

| feat | 实现功能 |
| --- | --- |
| fix | 缺陷修复 |
| docs | 完善文档 |
| style | 格式化代码风格 |
| refactor | 仅重构，不改动功能 |
| test | 增加、重构、修复测试，不改动功能代码 |
| chore | 其他小的修复 |

<a name="vOqZ0"></a>

## subject

需要简短的描述做了件什么事
对于包含复杂代码变更的 commit，应当在 commit 描述中给出详细说明
对于使用了 Monorepo 的项目，需要在对应的 commit message 中指明所涉及到的组件名或子项目名
