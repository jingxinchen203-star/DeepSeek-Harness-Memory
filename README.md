# DeepSeek-Harness-Memory

![DeepSeek Harness Memory project hero](assets/project-hero.png)


![License](https://img.shields.io/badge/license-MIT-blue)
![Node](https://img.shields.io/badge/node-%3E%3D22.19.0-brightgreen)
![pnpm](https://img.shields.io/badge/pnpm-11.7.0-blue)

**DeepSeek Harness with Memory Plugin** — 基于 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 构建，内置高优先级记忆管理能力。

---

## 什么是 DeepSeek Harness

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 是由 DeepSeek AI 开发的开源 Agent Harness。它采用 **Plugin-First** 架构，所有能力都通过插件扩展，底层由 [Cordis](https://github.com/cordiverse/cordis) 提供运行时支持。

---

## 本项目新增：Memory Plugin

在原版 DeepSeek Harness 基础上，本项目集成了 **Memory Plugin**，为 Agent 提供持久化的高优先级记忆能力。

### 核心特性

| 特性 | 说明 |
|------|------|
| **高优先级记忆注入** | 记忆内容通过 `systemPrompt.section` 注入，优先级高于普通对话上下文 |
| **导入 / 导出** | 支持 JSON / Markdown / 纯文本导入，支持导出为标准 JSON |
| **置顶与排序** | 关键记忆可置顶，确保在系统提示中优先展示 |
| **工作盘偏好** | 内置工作盘优先级规则：`E 盘 > D 盘 > C 盘`，避免在低容量盘工作 |
| **实时同步** | 设置页中的记忆变更会实时同步到 Agent 上下文 |

### 记忆优先级机制

Memory Plugin 通过以下方式确保记忆被优先遵循：

```
order: 4  → 工作盘偏好规则（E > D > C）
order: 5  → 用户记忆内容（MUST 优先遵循）
```

这意味着：
- 记忆内容会出现在系统提示的靠前位置
- 后续对话中，模型会优先遵循记忆中的指令
- 工作盘偏好会被模型在文件操作、代码生成时优先考虑

---

## 快速开始

### 环境要求

- Node.js `>= 22.19.0` 或 `>= 24.0.0`
- pnpm `11.7.0`
- Git

### 克隆仓库

```bash
git clone https://github.com/jingxinchen203-star/DeepSeek-Harness-Memory.git
cd DeepSeek-Harness-Memory
```

### 安装依赖

```bash
pnpm install
```

### 构建

```bash
pnpm run build
```

### 启动 Web UI

```bash
pnpm dsh web
```

启动后访问 `http://127.0.0.1:3080`，在设置页的「记忆」标签中即可使用记忆管理功能。

---

## 功能详解

### 1. 记忆管理

在设置页「记忆」中，你可以：

- **导入记忆**：点击「导入记忆」选择 `.json` / `.md` / `.txt` 文件
- **导出记忆**：点击「导出记忆」下载当前所有记忆为 `memories.json`
- **删除记忆**：单条删除或「清空全部」
- **置顶记忆**：重要记忆可置顶，确保在系统提示中排在最前

### 2. 工作盘偏好

项目已内置工作盘优先级规则：

```
优先：E 盘
其次：D 盘
最低：C 盘（尽量避免）
```

该规则通过高优先级 system prompt 注入，Agent 在生成代码、执行文件操作时会自动遵守。

### 3. 记忆格式

**JSON 导入示例：**

```json
[
  {
    "content": "用户偏好使用 TypeScript 而不是 JavaScript",
    "source": "manual",
    "pinned": true
  },
  {
    "content": "所有输出必须使用中文",
    "source": "manual",
    "pinned": false
  }
]
```

**Markdown / 纯文本导入：**

直接导入 `.md` 或 `.txt` 文件，每条非空行会被视为一条独立记忆。

---

## 项目结构

```
├── packages/
│   └── extensions/
│       └── tool-cordis/
│           └── src/
│               └── index.ts          # Memory Plugin 核心实现
├── docs/                            # 架构与开发文档
├── apps/                            # 前端应用
├── examples/                        # 示例配置
├── vendor/                          # Cordis 运行时
└── README.md                        # 项目说明
```

---

## 开发

### 脚本说明

| 命令 | 说明 |
|------|------|
| `pnpm run build` | 构建 host + client 产物 |
| `pnpm run typecheck` | TypeScript 类型检查 |
| `pnpm run lint` | 代码检查 |
| `pnpm run test` | 运行测试套件 |
| `pnpm dsh web` | 启动 Web UI |

### 插件开发

Memory Plugin 是一个动态 Cordis Plugin，核心代码位于：

- `packages/extensions/tool-cordis/src/index.ts`

如需修改记忆逻辑或 UI，直接编辑该文件后重启即可。

---

## 与官方版本的区别

| 特性 | deepseek-ai/deepseek-harness | 本项目 |
|------|------------------------------|--------|
| Memory Plugin | ❌ 未内置 | ✅ 内置 |
| 记忆优先级注入 | ❌ | ✅ order: 5 |
| 工作盘偏好 | ❌ | ✅ order: 4 |
| 记忆导入导出 | ❌ | ✅ JSON/MD/TXT |
| 记忆置顶排序 | ❌ | ✅ 支持 |

---

## 贡献

欢迎提交 Issue 和 Pull Request。

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'feat: add amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 开启 Pull Request

---

## 许可证

本项目采用 [MIT](LICENSE) 许可证。

---

## 致谢

- [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) — 基础框架
- [Cordis](https://github.com/cordiverse/cordis) — 插件运行时

---

<p align="center">Made with ❤️ by DeepSeek Harness Community</p>
