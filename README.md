# Clawd Code

> 本项目仅供学习与研究用途，Claude Code 的一切权利归 [Anthropic](https://www.anthropic.com/) 所有。


## 技术栈

| 类别 | 技术 |
|------|------|
| 运行时 | [Bun](https://bun.sh/) |
| 开发语言 | TypeScript |
| UI 框架 | React 19 + Ink（终端 UI）|
| AI SDK | `@anthropic-ai/sdk` |
| 通信协议 | MCP（Model Context Protocol）|
| 多云支持 | AWS Bedrock、Google Vertex AI、Azure |

## 快速开始

### 安装依赖

```bash
bun install
```

### 开发运行

```bash
bun run dev
```

### 生产构建

```bash
bun run build
```

> 构建产物将输出至 `dist/` 目录。

## 项目结构

```
src/
├── entrypoints/     # 程序入口
├── commands/        # CLI 命令
├── tools/           # AI 工具集
├── components/      # UI 组件
├── hooks/           # React Hooks
├── services/        # 业务服务层
├── bridge/          # 会话桥接层
├── utils/           # 工具函数
└── main.tsx         # 主程序
```

## 免责声明

本项目仅用于逆向学习与技术研究，**严禁用于任何商业用途**。Claude Code 及相关商标均归 [Anthropic](https://www.anthropic.com/) 所有。
