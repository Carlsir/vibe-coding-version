---
name: "ai-coding-init"
description: "AI编码项目初始化。首次使用时扫描文件、建立版本基线、创建目录结构和配置文件。检测到Git时让渡。Invoke when user starts AI coding project without Git, needs initialization, or says 'init version management'."
---

# AI 编码项目初始化

## 触发条件

- 用户说"初始化版本管理"、"设置版本管理"、"init"
- Agent 检测到项目中无 CHANGELOG.md 且无 .git

## 执行步骤

### 1. Git 让渡检测

```
IF 项目根目录存在 .git/ →
  输出："检测到 Git，版本管理由 Git 负责，本 Skill 不干预"
  退出，不执行任何操作
```

### 2. 扫描项目文件

扫描项目根目录下所有文件，排除默认忽略模式：
- `temp/*`, `*.log`, `*.tmp`, `node_modules/`, `.env`, `__pycache__/`, `*.pyc`, `dist/`, `build/`, `.trae/`

记录扫描结果：文件数量 N。

### 3. 创建目录结构

```
project-root/
├── current/              # 当前工作区
├── versions/             # 历史版本归档
│   ├── code/
│   ├── doc/
│   ├── config/
│   └── log/
├── stable/               # 已验证可运行版本快照
├── CHANGELOG.md          # 文件版本日志
├── PROJECT_STATE.md      # 项目口径文件
└── .ai-version           # 配置文件
```

### 4. 创建 CHANGELOG.md

```markdown
# 变更日志

| 时间 | 文件 | 版本 | 修改类型 | 修改摘要 | AI模型 | 可运行 |
|------|------|------|---------|---------|--------|--------|
| {当前时间} | 基线导入 | v0 | 初始化 | 扫描导入{N}个文件 | - | ⚠️未验证 |
```

### 5. 创建 PROJECT_STATE.md

```markdown
# 项目状态

## 基本信息
- 项目名：{项目目录名}
- 创建时间：{当前日期}
- 当前版本：v0
- 最后可运行版本：无

## 当前状态
基线已建立，共 {N} 个文件。版本管理已初始化。

## 关键决策
（暂无）

## 下次建议
验证当前文件是否可正常运行。验证通过后标记为可运行版本，然后开始正式开发。
```

### 6. 创建 .ai-version

```ini
[ignore]
temp/*
*.log
*.tmp
node_modules/
.env
__pycache__/
*.pyc
dist/
build/
.trae/

[config]
skill_version=1.0
max_versions=5
```

### 7. 输出确认

```
版本管理已初始化。
基线版本 v0，共 {N} 个文件。
建议先验证文件可运行性，然后开始开发。
```