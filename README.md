# OpenCode 对话记忆同步系统

## 📖 目录
1. [这是什么？](#这是什么)
2. [部署前准备](#部署前准备)
3. [详细部署步骤](#详细部署步骤)
4. [使用方法](#使用方法)
5. [常见问题](#常见问题)
6. [文件说明](#文件说明)

---

## 这是什么？

这是一个让 **OpenCode 对话记忆自动同步到云端** 的系统。

### 能做什么？
- ✅ 自动保存每一条对话记录到云端服务器
- ✅ 下次打开 OpenCode 时自动加载历史对话
- ✅ 支持跨设备同步（手机、平板、电脑都能看到）
- ✅ 防止对话丢失，电脑重装也不怕

### 工作原理
```
你的电脑 → 云端服务器 → 任何设备都可以读取
     ↑                    ↓
  每20秒自动上传        加载历史记录
```

---

## 部署前准备

### 需要的东西
| 项目 | 说明 | 是否必须 |
|------|------|---------|
| OpenCode | AI编程助手 | ✅ 必须 |
| Node.js | 运行脚本的环境 | ✅ 必须 |
| 网络连接 | 访问云端服务器 | ✅ 必须 |
| 云端服务器 | 存储对话记录 | ✅ 已有（已配置好） |

### 检查 Node.js
按 `Win + R`，输入 `cmd`，回车，输入：
```
node --version
```
如果显示版本号（如 v20.x.x），说明已安装。

**如果没有安装**：访问 https://nodejs.org ，下载 LTS 版本安装。

---

## 详细部署步骤

### 第一步：打开配置文件目录

1. 按 `Win + R`
2. 输入：`%USERPROFILE%\.config\opencode`
3. 回车

这会打开 OpenCode 的配置文件夹。

### 第二步：复制文件

从本压缩包中复制以下文件到配置目录：

| 文件名 | 复制到 |
|--------|--------|
| `load-memory-context.js` | 配置目录根目录 |
| `memory-watcher.js` | 配置目录根目录 |
| `db-query.py` | 配置目录根目录 |
| `opencode-with-memory.bat` | 配置目录根目录 |
| `skills/memory-sync/SKILL.md` | `skills/memory-sync/` 目录 |
| `skills/memory-sync/memory-sync.json` | `skills/memory-sync/` 目录 |

**skills 目录结构：**
```
配置目录/
├── skills/
│   └── memory-sync/
│       ├── SKILL.md
│       └── memory-sync.json
```

### 第三步：修改配置文件

1. 用记事本打开 `oh-my-openagent.jsonc`
2. 找到 `sisyphus` 配置项
3. 添加 `"skills": ["memory-sync"]`

**示例：**
```json
"sisyphus": { 
  "model": "claude/claude-opus-4-6",
  "temperature": 0.1,
  "skills": ["memory-sync"]
},
```

### 第四步：测试运行

1. 双击 `opencode-with-memory.bat`
2. 等待加载完成
3. 看到"启动完成"后，OpenCode 会自动打开

---

## 使用方法

### 方式一：一键启动（推荐）

双击 `opencode-with-memory.bat`

系统会自动：
1. 加载历史对话
2. 启动后台同步程序
3. 打开 OpenCode

### 方式二：单独使用

**加载历史：**
```bash
node C:\Users\你的用户名\.config\opencode\load-memory-context.js --load
```

**手动同步：**
```bash
node C:\Users\你的用户名\.config\opencode\load-memory-context.js --sync user "消息内容"
```

---

## 常见问题

### Q1：提示"无法连接服务器"
- 检查网络是否正常
- 确认服务器地址是否正确
- 可能是服务器维护，稍后再试

### Q2：历史记录显示乱码
- 不影响使用，功能正常
- 这是编码问题，可以忽略

### Q3：同步不工作
- 检查 `watcher.log` 日志文件
- 尝试重启 `opencode-with-memory.bat`

### Q4：如何停止同步
关闭标题为 `memory-watcher` 的窗口即可。

---

## 文件说明

| 文件 | 作用 |
|------|------|
| `load-memory-context.js` | 加载历史消息、同步单条消息 |
| `memory-watcher.js` | 后台守护进程，自动同步新消息 |
| `db-query.py` | 查询本地数据库 |
| `opencode-with-memory.bat` | 一键启动脚本 |
| `SKILL.md` | 告诉 AI 如何使用同步功能 |
| `memory-sync.json` | 技能配置文件 |
| `.memory-context.md` | 运行时生成的上下文文件 |
| `.memory-history.txt` | 运行时生成的历史记录 |

---

## 技术支持

如有问题，请提供：
1. `watcher.log` 的内容
2. 错误提示截图
3. 操作步骤描述

---

*最后更新：2026-04-27*
