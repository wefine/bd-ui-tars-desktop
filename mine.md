# bd-ui-tars-desktop 项目结构分析

## 项目概述

这是一个基于 Electron 的桌面应用程序项目，主要专注于 UI 自动化和 AI 代理技术。项目采用 pnpm workspace 的单仓多包(monorepo)架构。

## 目录结构详解

### 根目录结构
```
bd-ui-tars-desktop/
├── apps/                 # 应用程序目录
├── packages/             # 共享包目录
├── multimodal/           # 多模态AI代理核心模块
├── examples/             # 示例代码
├── docs/                 # 文档
├── scripts/              # 构建和发布脚本
├── patches/              # 补丁文件
└── 配置文件              # package.json, tsconfig.json 等
```

### 1. apps/ - 应用程序目录

#### apps/ui-tars/ - 主桌面应用
基于 Electron 构建的桌面应用程序。

**技术栈：**
- Electron + Vite
- TypeScript
- React (renderer 进程)
- Node.js (main 进程)

**主要文件结构：**
```
ui-tars/
├── src/
│   ├── main/            # Electron 主进程代码 (56个文件)
│   ├── preload/         # 预加载脚本
│   └── renderer/        # React 渲染进程代码 (107个文件)
├── build/               # 构建配置
├── e2e/                 # 端到端测试
├── images/              # 应用截图和资源
├── resources/           # 应用图标和静态资源
├── scripts/             # 构建脚本
└── 配置文件             # electron-builder.yml, vite.config 等
```

### 2. packages/ - 共享包目录

项目采用模块化设计，将核心功能拆分为独立的包：

#### packages/agent-infra/ - 代理基础设施
```
agent-infra/
├── browser/             # 浏览器操作模块
├── browser-use/         # 浏览器使用工具
├── create-new-mcp/      # MCP 创建工具
├── logger/              # 日志模块
├── mcp-benchmark/       # MCP 基准测试
├── mcp-client/          # MCP 客户端
├── mcp-http-server/     # MCP HTTP 服务器
├── mcp-servers/         # MCP 服务器集合
├── mcp-shared/          # MCP 共享工具
└── shared/              # 共享工具
```

#### packages/common/ - 通用配置
```
common/
├── configs/             # 通用配置文件
└── electron-build/      # Electron 构建工具
```

#### packages/ui-tars/ - UI Tars 核心包
```
ui-tars/
├── action-parser/       # 动作解析器
├── cli/                 # 命令行工具
├── electron-ipc/        # Electron 进程间通信
├── operators/           # 操作符集合
├── sdk/                 # SDK 包
├── shared/              # 共享工具
├── utio/                # UI 测试工具
└── visualizer/          # 可视化工具
```

### 3. multimodal/ - 多模态 AI 代理模块

这是项目的核心 AI 功能模块：

#### multimodal/agent-tars/ - Agent Tars 核心
```
agent-tars/
├── cli/                 # 命令行接口
├── core/                # 核心逻辑 (78个文件)
└── interface/           # 接口定义
```

#### multimodal/gui-agent/ - GUI 代理
图形界面自动化代理：
```
gui-agent/
├── action-parser/       # 动作解析
├── agent-sdk/           # 代理 SDK
├── operator-*           # 各种操作符 (adb, aio, browser, nutjs)
└── shared/              # 共享工具
```

#### multimodal/omni-tars/ - Omni Tars
全能 AI 代理：
```
omni-tars/
├── code-agent/          # 代码代理
├── core/                # 核心逻辑
├── gui-agent/           # GUI 代理
├── mcp-agent/           # MCP 代理
└── omni-agent/          # 全能代理
```

#### multimodal/tarko/ - Tarko 框架
大型 AI 代理框架：
```
tarko/
├── agent/               # 代理核心 (212个文件)
├── agent-cli/           # 代理 CLI
├── agent-interface/     # 代理接口
├── agent-server/        # 代理服务器
├── agent-server-next/   # Next.js 服务器
├── agent-snapshot/      # 快照工具
├── agent-ui/            # 代理 UI (165个文件)
├── agent-ui-builder/    # UI 构建器
├── agent-ui-cli/        # UI CLI
├── config-loader/       # 配置加载器
├── context-engineer/    # 上下文工程师
├── interface/           # 接口定义
├── llm/                 # 大语言模型集成
├── llm-client/          # LLM 客户端
├── mcp-agent/           # MCP 代理
├── model-provider/      # 模型提供商
├── pnpm-toolkit/        # pnpm 工具包
├── shared-media-utils/  # 媒体工具
├── shared-utils/        # 共享工具
└── ui/                  # UI 组件库
```

#### multimodal/benchmark/ - 基准测试
```
benchmark/
└── content-extraction/   # 内容提取测试
```

#### multimodal/websites/ - 网站文档
```
websites/
├── docs/                # 文档网站 (200+文件)
├── main/                # 主网站
└── tarko/               # Tarko 文档网站
```

### 4. examples/ - 示例代码
```
examples/
├── conditional-visibility-settings.config.ts
├── enhanced-runtime-settings.config.ts
├── operator-browserbase/    # Browserbase 操作符示例
└── presets/                 # 预设配置
```

### 5. docs/ - 文档
包含快速开始指南、部署文档、SDK 文档等。

### 6. scripts/ - 脚本工具
```
scripts/
├── merge-yml/           # YAML 合并工具
├── release-beta-pkgs.sh # Beta 包发布脚本
└── release-pkgs.sh      # 正式包发布脚本
```

## 技术架构特点

### 1. 单仓多包 (Monorepo)
- 使用 pnpm workspace 管理多个包
- 统一的依赖管理和构建流程
- 模块化设计，便于代码复用

### 2. 多层架构
- **应用层**: Electron 桌面应用
- **服务层**: 各种 AI 代理服务
- **工具层**: 操作符、解析器、工具包
- **基础设施层**: MCP 协议、日志、配置等

### 3. AI 代理生态
- 支持多种类型的代理：GUI 代理、代码代理、MCP 代理
- 多模态支持：文本、图像、动作等
- 插件化架构：通过操作符扩展功能

### 4. 跨平台支持
- Electron 确保 Windows/macOS 兼容性
- 容器化部署支持

## 主要功能模块

1. **UI 自动化**: 通过视觉识别和动作解析实现界面自动化
2. **AI 代理**: 集成多种大语言模型，提供智能代理服务
3. **MCP 协议**: 支持 Model Context Protocol，扩展 AI 能力
4. **桌面应用**: 提供用户友好的图形界面
5. **开发者工具**: CLI 工具、SDK、可视化工具等

## 启动指南

### 环境要求

- **Node.js**: >= 20.x
- **包管理器**: pnpm >= 9.10.0
- **浏览器**: Chrome/Edge/Firefox (用于浏览器操作)
- **操作系统**: macOS/Windows (单显示器设置)

### 安装依赖

```bash
# 安装项目依赖
pnpm bootstrap
```

**`pnpm bootstrap` 命令详解：**

这是项目定义的一个 npm script，实际执行 `pnpm i` (pnpm install) 命令。在当前这个 **pnpm workspace 单仓多包(monorepo)** 项目中，该命令会：

1. **读取 workspace 配置**
   - 根据 `pnpm-workspace.yaml` 文件识别所有子包
   - 包括 `apps/*`、`packages/*` 等目录下的包

2. **安装所有依赖**
   - 安装根目录的公共依赖
   - 递归安装所有 workspace 子包的依赖
   - 解析和安装跨包依赖关系

3. **创建包间链接**
   - 使用符号链接(symlinks)连接 workspace 内的包
   - 使 `@ui-tars/*`、`@common/*` 等内部包可以相互引用

4. **依赖去重**
   - pnpm 的优势：相同的依赖包只安装一次，通过硬链接复用
   - 大大减少 node_modules 的体积

**使用时机：**
- **首次克隆项目后**：运行 `pnpm bootstrap` 初始化环境
- **添加新依赖后**：重新运行以更新依赖树
- **修改 workspace 配置后**：需要重新 bootstrap

### 开发模式启动

```bash
# 启动 UI-TARS Desktop 开发环境
pnpm dev:ui-tars

# 或者直接在 ui-tars 目录下运行
cd apps/ui-tars
pnpm dev
```

### 调试模式

```bash
# 带源码映射的调试模式
pnpm debug

# 带源码映射的 watch 模式
pnpm debug:w
```

### 生产构建

```bash
# 构建生产版本
pnpm build

# 打包为可执行文件
pnpm make
```

### 本地操作符配置

1. **下载并安装应用**
   - 从 [GitHub Releases](https://github.com/bytedance/UI-TARS-desktop/releases/latest) 下载最新版本
   - 或者使用 Homebrew: `brew install --cask ui-tars`

2. **macOS 权限设置**
   - 系统设置 → 隐私与安全性 → 辅助功能
   - 系统设置 → 隐私与安全性 → 屏幕录制

3. **模型配置**
   - **选项1**: 使用 Hugging Face 部署 UI-TARS-1.5
     - 访问 [Hugging Face Endpoints](https://endpoints.huggingface.co/catalog)
     - 部署 UI-TARS-1.5-7B 模型
     - 在应用设置中配置 Base URL 和 API Key

   - **选项2**: 使用 VolcEngine Doubao-1.5-UI-TARS
     - 访问 [VolcEngine 控制台](https://console.volcengine.com/ark/region:ark+cn-beijing/model/detail?Id=doubao-1.5-ui-tars)
     - 获取 API Key 和 Base URL
     - 在应用设置中配置相应参数

4. **设置配置**
   ```yaml
   # Hugging Face 配置
   Language: en
   VLM Provider: Hugging Face for UI-TARS-1.5
   VLM Base URL: https://xxx/v1/
   VLM API KEY: your_api_key
   VLM Model Name: ui-tars-1.5-7b

   # 或 VolcEngine 配置
   Language: cn
   VLM Provider: VolcEngine Ark for Doubao-1.5-UI-TARS
   VLM Base URL: https://ark.cn-beijing.volces.com/api/v3
   VLM API KEY: YOUR_API_KEY
   VLM Model Name: doubao-1.5-ui-tars-250328
   ```

### 远程操作符

> 注意: 远程操作符服务将于 2025年8月20日停止。如需继续使用，可部署自己的远程计算机和浏览器代理。

- 访问 Volcano Engine 的 OS Agent Services
- 部署 [Computer Use Agent](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/application/create?templateId=680b0a890e881f000862d9f0)
- 部署 [Browser Use Agent](https://console.volcengine.com/vefaas/region:vefaas+cn-beijing/application/create?templateId=67f7b4678af5a6000850556c&channel=github&source=ui-tars)

## 开发和构建

- **包管理**: pnpm workspace
- **构建工具**: Vite, Electron Builder
- **测试**: Vitest, Playwright (E2E)
- **代码质量**: TypeScript, ESLint
- **CI/CD**: 支持自动发布和部署

这个项目是一个功能丰富、架构复杂的 AI 驱动的 UI 自动化平台，整合了桌面应用、AI 代理、多模态交互等多种技术。
