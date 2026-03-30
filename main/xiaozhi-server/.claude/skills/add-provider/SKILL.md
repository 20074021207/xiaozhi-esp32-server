---
name: add-provider
description: 添加新的 AI 服务 Provider（ASR/LLM/TTS/VAD/Memory/Intent）
---

# 添加新 Provider

为 xiaozhi-server 添加新的 AI 服务提供者实现。

## 步骤

1. **确定 Provider 类型**：ASR、LLM、TTS、VAD、Memory 或 Intent
2. **阅读基类**：读取 `core/providers/<type>/base.py`，理解需要实现的抽象方法
3. **参考现有实现**：阅读同目录下的 1-2 个现有实现作为模板
4. **创建实现文件**：在 `core/providers/<type>/` 下创建新文件
5. **实现抽象方法**：确保所有标注了 `@abstractmethod` 的方法都被实现
6. **注册 Provider**：在 `core/providers/<type>/__init__.py` 中添加导入和注册
7. **添加配置**：在 `config.yaml` 中添加该 Provider 的配置模板
8. **更新 selected_module**：在 config.yaml 的 `selected_module` 中添加可选项
9. **验证**：运行 `python app.py` 确认无导入错误

## 注意事项

- 所有 Provider 核心方法必须是 `async`
- 使用 `logger.bind(tag=TAG)` 记录日志
- 配置项从 `self.config` 读取（在 `__init__` 中传入）
- 如果需要额外依赖，更新 `requirements.txt`
