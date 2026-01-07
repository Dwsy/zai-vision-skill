---
name: zai-skill
description: 智谱 AI 多模态视觉分析能力，提供 UI 转代码、OCR、错误诊断、技术图表理解等 8 个工具
---

# zai-mcp Skill

智谱 AI 多模态视觉分析技能，提供从 UI 截图到代码生成、OCR 文字提取、错误诊断、技术图表理解等全方位的视觉分析能力。

## Context Efficiency

传统 MCP 方式：
- 所有 8 个工具在启动时加载
- 预估上下文消耗：4000+ tokens

本技能方式：
- 元数据仅：~150 tokens
- 完整指令（使用时）：~6k tokens
- 工具执行：0 tokens（外部运行）

## Available Tools

### UI & Design Tools

**`ui_to_artifact`** - 将 UI 截图转换为代码、提示词、设计规范或自然语言描述

参数：
- `image_source` (string, required): 本地文件路径或远程 URL
- `output_type` (string, required): 输出类型 - `code`/`prompt`/`spec`/`description`
- `prompt` (string, required): 详细指令，说明要生成什么

**`ui_diff_check`** - 对比两张 UI 截图，识别视觉差异和实现偏差

### Text & Code Extraction

**`extract_text_from_screenshot`** - 使用先进的 OCR 能力从截图中提取和识别文字

参数：
- `image_source` (string, required): 本地文件路径或远程 URL

### Error Diagnosis

**`diagnose_error_screenshot`** - 解析错误弹窗、堆栈和日志截图，给出定位与修复建议

参数：
- `image_source` (string, required): 本地文件路径或远程 URL

### Technical Diagrams

**`understand_technical_diagram`** - 针对架构图、流程图、UML、ER 图等技术图纸生成结构化解读

参数：
- `image_source` (string, required): 本地文件路径或远程 URL

### Data Analysis

**`analyze_data_visualization`** - 阅读仪表盘、统计图表，提炼趋势、异常与业务要点

参数：
- `image_source` (string, required): 本地文件路径或远程 URL

### General Analysis

**`analyze_image`** - 通用图像理解能力，适配未被专项工具覆盖的视觉内容

参数：
- `image_source` (string, required): 本地文件路径或远程 URL

**`analyze_video`** - 支持 MP4/MOV/M4V（限制本地最大 8M）等格式的视频场景解析

参数：
- `video_source` (string, required): 本地文件路径或远程 URL

## Usage Pattern

当用户请求匹配本技能能力时：

**Step 1: 识别合适的工具** 从上面的列表中选择

**Step 2: 生成工具调用** 使用 JSON 格式：

```json
{
  "tool": "tool_name",
  "arguments": {
    "param1": "value1",
    "param2": "value2"
  }
}
```

**Step 3: 通过 bash 执行：**

```bash
cd $SKILL_DIR
python executor.py --call 'YOUR_JSON_HERE'
```

⚠️ **重要**：将 `$SKILL_DIR` 替换为实际发现的技能目录路径。

## Getting Tool Details

如果需要特定工具的详细参数信息：

```bash
cd $SKILL_DIR
python executor.py --describe tool_name
```

这只会加载该工具的 schema，不会加载所有工具。

## Examples

### Example 1: UI 截图转代码

用户: "将这个 UI 截图转换为 React 代码"

你的流程：
1. 识别工具：`ui_to_artifact`
2. 生成调用 JSON
3. 执行：

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "ui_to_artifact", "arguments": {"image_source": "/path/to/screenshot.png", "output_type": "code", "prompt": "Generate React code with Tailwind CSS for this UI"}}'
```

### Example 2: OCR 提取代码

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "extract_text_from_screenshot", "arguments": {"image_source": "/path/to/code_screenshot.png"}}'
```

### Example 3: 错误诊断

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "diagnose_error_screenshot", "arguments": {"image_source": "/path/to/error_screenshot.png"}}'
```

### Example 4: 技术图表理解

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "understand_technical_diagram", "arguments": {"image_source": "/path/to/architecture_diagram.png"}}'
```

### Example 5: UI 差异对比

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "ui_diff_check", "arguments": {"image_path_1": "/path/to/design.png", "image_path_2": "/path/to/implementation.png"}}'
```

### Example 6: 视频分析

```bash
cd $SKILL_DIR
python executor.py --call '{"tool": "analyze_video", "arguments": {"video_source": "/path/to/video.mp4"}}'
```

## Error Handling

如果 executor 返回错误：
- 检查工具名称是否正确
- 验证必需参数是否已提供
- 确保 MCP 服务器可访问
- 检查 API 密钥是否有效

## Performance Notes

上下文使用对比：

| 场景 | MCP (预加载) | Skill (动态) |
|------|--------------|--------------|
| 空闲 | 4000 tokens | 150 tokens |
| 活跃 | 4000 tokens | 6000 tokens |
| 执行中 | 4000 tokens | 0 tokens |

节省：典型使用中节省约 96% 的上下文

## Configuration

本技能使用智谱 AI API，配置：
- API Key: 已配置在 mcp-config.json
- 模式: ZHIPU

---

*本技能由 zai-mcp-server MCP 服务器配置自动生成*
*Generator: mcp_to_skill.py*