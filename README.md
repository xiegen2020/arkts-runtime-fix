# ArkTS Runtime Fix

> 面向 HarmonyOS / OpenHarmony 开发的 AI 编程技能包 —— ArkTS 运行时崩溃诊断

专门用于诊断和修复 ArkTS/JavaScript 运行时崩溃（jscrash），支持解析 crash log、faultlogger 日志和 hilog，定位 `TypeError`、`ReferenceError`、`BusinessError` 等运行时异常。适用于 Claude Code、DevEco Code、Cursor、GitHub Copilot、Windsurf 等 AI 编程工具。

## 触发场景

- 运行时日志显示 `TypeError`、`ReferenceError`、`RangeError`、`SyntaxError`、`BusinessError` 等异常
- 应用启动或点击操作后闪退、白屏
- 提供了 jscrash 日志、堆栈信息或 `@file` 临时日志
- 编译成功但运行时行为异常

## 诊断脚本

本技能内置了多个 Node.js 诊断脚本，可直接在终端中调用：

| 脚本 | 功能 |
|------|------|
| `scripts/jscrash-report.mjs` | 解析用户提供的 crash 文本 |
| `scripts/parse-jscrash-log.mjs` | 解析本地日志文件 |
| `scripts/probe-faultlogger.mjs` | 查询设备 faultlogger |
| `scripts/fetch-faultlog.mjs` | 拉取设备 faultlog |
| `scripts/collect-hilog.mjs` | 收集设备 hilog |

## 常见崩溃签名

| 签名 | 典型原因 | 修复方向 |
|------|---------|---------|
| `TypeError` 属性访问 | 渲染或生命周期中 null/undefined | 守卫空状态，提前初始化 |
| `ReferenceError` | 作用域错误、过期导入、缺失符号 | 修复符号归属或导入路径 |
| `RangeError` | 无效索引、递归循环 | 添加边界检查 |
| `BusinessError` | 框架 API 前置条件未满足 | 校验参数、权限或调用时机 |

## 安装

### Claude Code / DevEco Code

```bash
npx skills add xiegen2020/arkts-runtime-fix
```

或手动复制到技能目录：

```bash
# Claude Code
cp -r arkts-runtime-fix ~/.claude/skills/

# DevEco Code
cp -r arkts-runtime-fix ~/.config/deveco/skills/
```

### Cursor

将 `SKILL.md` 内容复制到项目的 `.cursor/rules/arkts-runtime-fix.mdc` 文件中。

### 其他工具

将 `SKILL.md` 放入对应工具的技能/规则配置目录即可。注意：诊断脚本需要 Node.js 环境才能运行。

## 目录结构

```
arkts-runtime-fix/
├── SKILL.md              # 技能主文件（英文）
├── SKILL_CN.md           # 技能主文件（中文）
├── README.md
├── scripts/              # 诊断脚本（.mjs 可执行 + .ts 源码）
│   ├── jscrash-report.mjs
│   ├── parse-jscrash-log.mjs
│   ├── probe-faultlogger.mjs
│   ├── fetch-faultlog.mjs
│   ├── collect-hilog.mjs
│   └── shared/           # 共享模块
└── evals/                # 评测数据
    └── evals.json
```

## 来源

本技能提取自 [DevEco Code](https://gitcode.com/openharmony-sig/deveco-code) 开源项目（MIT 协议），由华为官方内置的 HarmonyOS 开发技能包。

## 相关技能

- [arkts-error-fixes](https://github.com/xiegen2020/arkts-error-fixes) — 编译错误修复
- [arkts-grammar-standards](https://github.com/xiegen2020/arkts-grammar-standards) — ArkTS 语法规范
- [arkui-knowledge](https://github.com/xiegen2020/arkui-knowledge) — ArkUI 组件知识库
- [deveco-create-project](https://github.com/xiegen2020/deveco-create-project) — 项目脚手架

## License

MIT
