---
name: add-plugin
description: 添加新的插件函数到 plugins_func/functions/
---

# 添加新插件函数

为 xiaozhi-server 添加可被 AI 调用的自定义功能插件。

## 步骤

1. **阅读注册机制**：读取 `plugins_func/register.py`，理解 `register_function` 装饰器和 `ToolType` 枚举
2. **参考现有插件**：阅读 `plugins_func/functions/` 下的 1-2 个现有实现（如 `get_weather.py`）
3. **创建插件文件**：在 `plugins_func/functions/` 下创建新 `.py` 文件
4. **使用装饰器注册**：
```python
from plugins_func.register import register_function, ToolType, Action, ActionResponse

@register_function("function_name", "函数功能描述", ToolType.SYSTEM_CTL)
async def my_function(conn, location=None, **kwargs):
    # conn 是 ConnectionHandler 实例
    # 实现功能逻辑
    return ActionResponse(Action.REQLLM, result, None)
```
5. **无需手动导入**：`loadplugins.py` 会自动发现和加载 `functions/` 目录下的所有插件
6. **验证**：启动服务器，确认日志中出现插件加载信息

## ToolType 说明

- `NONE`：简单函数调用
- `WAIT`：等待函数返回
- `SYSTEM_CTL`：系统控制，影响流程
- `IOT_CTL`：IoT 设备控制
- `CHANGE_SYS_PROMPT`：修改系统 prompt
- `MCP_CLIENT`：MCP 协议客户端

## Action 说明

- `Action.REQLLM`：需要 LLM 继续处理
- `Action.NORMAL`：直接返回结果
- `Action.NOTFOUND`：功能未找到
