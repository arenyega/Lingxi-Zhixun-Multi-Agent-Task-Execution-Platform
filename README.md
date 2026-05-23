# 灵犀智讯 Multi-Agent Task Execution Platform

> 一个基于 **Gradio + browser-use + Playwright + LangGraph** 的多 Agent 任务执行平台，支持浏览器自动化任务、深度研究任务、多模型配置、MCP 工具扩展以及 Docker 可视化部署。

![Web UI](assets/web-ui.png)

## 项目简介

**灵犀智讯 Multi-Agent Task Execution Platform** 是一个面向 AI Agent 实验与应用开发的 WebUI 平台。项目围绕“让 AI 能够理解任务、操作浏览器、调用工具并生成结果”展开，提供了可视化配置界面和多种 Agent 执行模式。

平台主要包含两类核心能力：

1. **Browser Use Agent**  
   让大模型通过浏览器完成网页访问、信息提取、表单填写、文件上传、页面交互等任务。

2. **Deep Research Agent**  
   面向复杂研究型任务，自动生成研究计划，调用多个浏览器子任务并行检索信息，最终汇总生成 Markdown 研究报告。

项目适合用于：

- AI Agent 原型开发
- 浏览器自动化任务实验
- 多模型能力对比
- Deep Research / 自动调研流程
- MCP 工具接入测试
- 多 Agent 协作任务执行平台搭建

---

## 功能特性

### 1. 可视化 WebUI

项目使用 Gradio 构建 Web 界面，提供以下配置页：

- Agent Settings：配置大模型 Provider、模型名、API Key、Base URL、温度等参数
- Browser Settings：配置浏览器路径、用户数据目录、窗口大小、是否 Headless、是否复用浏览器等
- Run Agent：输入自然语言任务，让 Agent 自动执行
- Agent Marketplace / Deep Research：运行深度研究任务并生成报告
- Load & Save Config：保存和加载 WebUI 配置

### 2. 多模型 Provider 支持

项目支持多种 LLM Provider，包括：

- OpenAI
- Azure OpenAI
- Anthropic
- DeepSeek
- Google Gemini
- Ollama
- Mistral
- Alibaba / 通义千问
- ModelScope
- Moonshot
- Unbound AI
- IBM Watsonx
- Grok
- SiliconFlow

你可以通过 `.env` 或 WebUI 页面填写 API Key、Endpoint 和模型名。

### 3. Browser Use Agent

Browser Use Agent 基于 `browser-use` 和 Playwright，能够让 AI 在真实浏览器环境中执行任务，例如：

- 打开网页
- 搜索信息
- 点击页面元素
- 输入文本
- 上传文件
- 提取网页正文
- 在必要时向用户请求帮助
- 输出任务历史 JSON
- 生成任务执行 GIF 记录

### 4. Deep Research Agent

Deep Research Agent 基于 LangGraph 组织研究流程，支持：

- 根据用户主题生成研究计划
- 拆分多个研究子任务
- 并行启动浏览器 Agent 搜索信息
- 保存研究过程文件
- 汇总生成 `report.md`
- 支持任务中断和 Resume Task ID 继续执行
- 支持下载最终 Markdown 报告

默认研究结果保存目录为：

```text
./tmp/deep_research
```

### 5. MCP 工具扩展

项目支持上传 MCP Server JSON 配置，并将 MCP 工具注册到 Agent Controller 中，从而扩展 Agent 的工具调用能力。

示例配置结构：

```json
{
  "mcpServers": {
    "desktop-commander": {
      "command": "npx",
      "args": ["-y", "@wonderwhy-er/desktop-commander"]
    }
  }
}
```

### 6. Docker + noVNC 部署

项目内置 `Dockerfile`、`docker-compose.yml` 和 `supervisord.conf`，支持在容器中启动：

- Gradio WebUI：`7788`
- noVNC 浏览器可视化页面：`6080`
- VNC 服务：`5901`
- Chrome Debugging Port：`9222`

---

## 技术栈

| 模块 | 技术 |
|---|---|
| WebUI | Gradio |
| 浏览器自动化 | Playwright, browser-use |
| Agent 编排 | LangGraph |
| LLM 接入 | LangChain, LangChain OpenAI/Anthropic/Google/Ollama 等 |
| 工具扩展 | MCP |
| 后端语言 | Python 3.11 |
| 容器化 | Docker, Docker Compose |
| 可视化浏览器 | Xvfb, x11vnc, noVNC |

---

## 项目结构

```text
.
├── assets/
│   ├── web-ui.png
│   └── examples/
├── src/
│   ├── agent/
│   │   ├── browser_use/
│   │   │   └── browser_use_agent.py
│   │   └── deep_research/
│   │       └── deep_research_agent.py
│   ├── browser/
│   │   ├── custom_browser.py
│   │   └── custom_context.py
│   ├── controller/
│   │   └── custom_controller.py
│   ├── utils/
│   │   ├── config.py
│   │   ├── llm_provider.py
│   │   ├── mcp_client.py
│   │   └── utils.py
│   └── webui/
│       ├── interface.py
│       ├── webui_manager.py
│       └── components/
├── tests/
├── tmp/
├── .env.example
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── supervisord.conf
├── webui.py
└── LICENSE
```

---

## 快速开始：本地运行

### 1. 克隆项目

```powershell
git clone https://github.com/arenyega/Lingxi-Zixun-Multi-Agent-Task-Execution-Platform.git
cd "Lingxi-Zixun-Multi-Agent-Task-Execution-Platform"
```

### 2. 创建 Python 虚拟环境

Windows PowerShell：

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS / Linux：

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. 安装依赖

```powershell
pip install -r requirements.txt
```

### 4. 安装 Playwright Chromium

```powershell
playwright install chromium
```

### 5. 配置环境变量

复制示例配置：

```powershell
copy .env.example .env
```

然后编辑 `.env`，填入至少一个模型服务的 API Key，例如：

```env
OPENAI_ENDPOINT=https://api.openai.com/v1
OPENAI_API_KEY=your_openai_api_key

DEEPSEEK_ENDPOINT=https://api.deepseek.com
DEEPSEEK_API_KEY=your_deepseek_api_key

DEFAULT_LLM=openai
```

> 注意：不要把真实 `.env` 文件提交到 GitHub。项目 `.gitignore` 已默认忽略 `.env`。

### 6. 启动 WebUI

```powershell
python webui.py --ip 127.0.0.1 --port 7788
```

浏览器打开：

```text
http://127.0.0.1:7788
```

---

## Docker 部署

### 1. 准备环境变量

```powershell
copy .env.example .env
```

编辑 `.env`，填入模型 API Key 和需要的配置。

### 2. 构建并启动

```powershell
docker compose up --build
```

启动后访问：

```text
http://localhost:7788
```

如果需要查看容器内浏览器画面，可以访问 noVNC：

```text
http://localhost:6080
```

默认 VNC 密码来自 `.env` 中的：

```env
VNC_PASSWORD=youvncpassword
```

### 3. 后台运行

```powershell
docker compose up -d --build
```

### 4. 停止服务

```powershell
docker compose down
```

---

## 使用说明

### 运行普通浏览器 Agent

1. 打开 WebUI
2. 进入 **Agent Settings**
3. 选择 LLM Provider 和模型
4. 填写 API Key / Base URL
5. 进入 **Browser Settings**
6. 根据需要配置是否使用本地浏览器、是否 Headless、浏览器窗口大小等
7. 进入 **Run Agent**
8. 输入任务，例如：

```text
请打开 GitHub，搜索 browser-use 项目，并总结它的主要功能。
```

Agent 执行完成后，可以下载：

- Agent History JSON
- Task Recording GIF

### 运行 Deep Research Agent

1. 进入 **Agent Marketplace**
2. 选择 **Deep Research**
3. 输入研究任务，例如：

```text
调研 2025 年 AI Agent 框架的发展趋势，并生成一份结构化报告。
```

4. 设置并行 Agent 数量
5. 点击 Run
6. 等待生成研究计划和最终报告

最终报告会以 Markdown 形式展示，并可下载。

---

## 环境变量说明

常用环境变量如下：

| 变量名 | 说明 |
|---|---|
| `DEFAULT_LLM` | 默认模型 Provider |
| `OPENAI_API_KEY` | OpenAI API Key |
| `OPENAI_ENDPOINT` | OpenAI API Base URL |
| `DEEPSEEK_API_KEY` | DeepSeek API Key |
| `DEEPSEEK_ENDPOINT` | DeepSeek Endpoint |
| `GOOGLE_API_KEY` | Google Gemini API Key |
| `OLLAMA_ENDPOINT` | Ollama 本地服务地址 |
| `ALIBABA_API_KEY` | 通义千问 API Key |
| `MODELSCOPE_API_KEY` | ModelScope API Key |
| `SiliconFLOW_API_KEY` | SiliconFlow API Key |
| `BROWSER_PATH` | 自定义浏览器路径 |
| `BROWSER_USER_DATA` | 浏览器用户数据目录 |
| `USE_OWN_BROWSER` | 是否使用已有浏览器 |
| `KEEP_BROWSER_OPEN` | 是否在任务间保持浏览器打开 |
| `BROWSER_DEBUGGING_PORT` | 浏览器调试端口 |
| `RESOLUTION` | Docker 虚拟显示分辨率 |
| `VNC_PASSWORD` | VNC 密码 |

完整配置可参考：

```text
.env.example
```

---

## 测试

项目包含一些测试脚本，位于 `tests/` 目录：

```text
tests/
├── test_agents.py
├── test_controller.py
├── test_llm_api.py
└── test_playwright.py
```

可以根据需要运行指定测试文件：

```powershell
python tests/test_llm_api.py
python tests/test_playwright.py
```

部分测试依赖真实浏览器、LLM API Key 或 MCP 工具，请先确认 `.env` 配置完整。

---

## 注意事项

1. **不要提交敏感信息**  
   `.env`、API Key、Token、浏览器用户数据目录等不应提交到 GitHub。

2. **浏览器自动化存在不确定性**  
   页面结构变化、验证码、登录态失效、网络波动都可能影响 Agent 执行效果。

3. **Deep Research 可能产生较多模型调用**  
   并行 Agent 数量越高，消耗的 API Token 和浏览器资源越多。

4. **MCP 工具需要谨慎配置**  
   如果 MCP 工具具备文件系统或命令执行能力，请确认其权限范围，避免误操作。

5. **tmp 目录用于运行产物**  
   任务历史、研究报告、下载文件等通常会保存在 `tmp/` 下，建议不要提交到仓库。

---

## License

本项目基于 MIT License 发布，详情见 [LICENSE](LICENSE)。

---

## 致谢

本项目基于 browser-use、Playwright、LangChain、LangGraph、Gradio 等开源生态构建。
