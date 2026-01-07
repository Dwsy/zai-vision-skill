# zai-skill

智谱 AI 多模态视觉分析 Claude Skill，提供 UI 转代码、OCR、错误诊断、技术图表理解等 8 个强大的视觉分析工具。

## Features

- ✅ **UI 转代码**: 将 UI 截图转换为代码、提示词、设计规范
- ✅ **OCR 文字提取**: 从截图中提取代码、终端输出、文档文字
- ✅ **错误诊断**: 分析错误弹窗、堆栈和日志，提供修复建议
- ✅ **技术图表理解**: 解读架构图、流程图、UML、ER 图
- ✅ **数据可视化分析**: 分析仪表盘、统计图表，提炼趋势和异常
- ✅ **UI 差异对比**: 对比两张 UI 截图，识别视觉差异
- ✅ **通用图像分析**: 灵活理解任何视觉内容
- ✅ **视频分析**: 支持 MP4/MOV/M4V 格式（最大 8MB）

## Installation

```bash
cd ~/.pi/agent/skills/zai-vision

# 安装依赖（推荐使用 uv）
uv sync

# 或者手动安装依赖
pip install -r requirements.txt
```

## Configuration

复制示例配置并编辑：

```bash
cp mcp-config.example.json mcp-config.json
```

编辑 `mcp-config.json` 配置智谱 AI API Key：

```json
{
  "name": "zai-vision",
  "transport": "stdio",
  "command": "npx",
  "args": ["@z_ai/mcp-server"],
  "env": {
    "Z_AI_API_KEY": "your-api-key",
    "Z_AI_MODE": "ZHIPU"
  }
}
```

## Usage

### List all tools

```bash
cd ~/.pi/agent/skills/zai-vision
uv run executor.py --list
```

### Describe a tool

```bash
uv run executor.py --describe ui_to_artifact
```

### Call a tool

```bash
uv run executor.py --call '{
  "tool": "ui_to_artifact",
  "arguments": {
    "image_source": "/path/to/image.png",
    "output_type": "code",
    "prompt": "Generate React code with Tailwind CSS"
  }
}'
```

### Statistics and Logging

```bash
# 查看状态
uv run executor.py --status

# 查看统计
uv run executor.py --stats

# 查看日志
uv run executor.py --logs 100

# 按工具过滤日志
uv run executor.py --logs 100 --tool ui_to_artifact

# 重置统计
uv run executor.py --reset-stats
```

## Tools

| Tool | Description |
|------|-------------|
| `ui_to_artifact` | UI 截图转代码/提示词/规范/描述 |
| `extract_text_from_screenshot` | OCR 文字提取 |
| `diagnose_error_screenshot` | 错误诊断和修复建议 |
| `understand_technical_diagram` | 技术图表理解 |
| `analyze_data_visualization` | 数据可视化分析 |
| `ui_diff_check` | UI 差异对比 |
| `analyze_image` | 通用图像分析 |
| `analyze_video` | 视频分析（最大 8MB） |

## Examples

### Example 1: UI to Code

```bash
uv run executor.py --call '{
  "tool": "ui_to_artifact",
  "arguments": {
    "image_source": "/path/to/ui-screenshot.png",
    "output_type": "code",
    "prompt": "Generate React component with TypeScript and Tailwind CSS"
  }
}'
```

### Example 2: Extract Text

```bash
uv run executor.py --call '{
  "tool": "extract_text_from_screenshot",
  "arguments": {
    "image_source": "/path/to/code-screenshot.png"
  }
}'
```

### Example 3: Diagnose Error

```bash
uv run executor.py --call '{
  "tool": "diagnose_error_screenshot",
  "arguments": {
    "image_source": "/path/to/error-screenshot.png"
  }
}'
```

### Example 4: Analyze Diagram

```bash
uv run executor.py --call '{
  "tool": "understand_technical_diagram",
  "arguments": {
    "image_source": "/path/to/architecture-diagram.png"
  }
}'
```

### Example 5: Analyze Video

```bash
uv run executor.py --call '{
  "tool": "analyze_video",
  "arguments": {
    "video_source": "/path/to/video.mp4"
  }
}'
```

## Performance

| Scenario | MCP (preload) | Skill (dynamic) | Savings |
|----------|---------------|-----------------|---------|
| Idle | 4000 tokens | 150 tokens | 96% |
| Active | 4000 tokens | 5k tokens | - |
| Executing | 4000 tokens | 0 tokens | 100% |

## Features

- ✅ **渐进式加载**: 仅在需要时加载工具定义，节省 96% 上下文
- ✅ **统计追踪**: 记录所有工具调用、成功率和执行时间
- ✅ **日志管理**: 详细的调用日志，支持按工具过滤
- ✅ **多协议支持**: stdio/SSE/HTTP 传输协议
- ✅ **依赖管理**: 使用 uv 进行标准化依赖管理
- ✅ **可重现构建**: uv.lock 确保依赖版本一致性

## Requirements

- Python 3.10+
- uv (推荐) 或 pip
- 智谱 AI API Key

### 安装 uv

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## API Key

获取智谱 AI API Key:

1. 访问: https://open.bigmodel.cn/
2. 注册账号
3. 创建 API Key
4. 在 `mcp-config.json` 中配置

## Documentation

- [智谱 AI 官方文档](https://docs.bigmodel.cn/cn/coding-plan/mcp/vision-mcp-server)
- [MCP 规范](https://modelcontextprotocol.io)

## License

MIT

## Credits

Based on [@z_ai/mcp-server](https://www.npmjs.com/package/@z_ai/mcp-server) by Z.AI