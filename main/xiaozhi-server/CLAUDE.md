# xiaozhi-server

See @main/README.md for architecture overview and @docs/ for integration guides.

# Commands
- Run: `python app.py` (WebSocket :8000, HTTP :8003)
- Test: no unit tests. Manual WebSocket test via `test/test_page.html`. Performance tests in `performance_tester/`
- Install deps: `pip install -r requirements.txt`

# Locked dependencies (DO NOT upgrade)
- torch==2.2.2, numpy==1.26.4 (compatibility)
- websockets==14.2 (cozepy requires <15.0.0)

# Environment
- Python 3.10 required
- ffmpeg must be installed (not in requirements.txt, but required for audio)

# Config priority
`data/.config.yaml` > `config.yaml` > manager-api remote config
Copy `config.yaml` to `data/.config.yaml` for local overrides (git-ignored).

# Code conventions
- Logging: always use `logger.bind(tag=TAG).info/error(...)` with loguru, never bare `logger.info`
- `connection.py` (~1473 lines) is the core coordinator — read it fully before modifying connection logic
- Audio codec is Opus — both device and server must stay consistent

# Provider pattern
Each AI service type (ASR/LLM/TTS/VAD/Memory/Intent) lives in `core/providers/<type>/`:
1. Inherit the base class (e.g. `ASRProviderBase`)
2. Implement abstract methods
3. Register in `core/providers/<type>/__init__.py`
4. Set in `config.yaml` under `selected_module`

# Plugin pattern
Use `@register_function` decorator in `plugins_func/functions/`. Plugins auto-loaded by `loadplugins.py`.
```python
@register_function("name", "description", ToolType.SYSTEM_CTL)
async def my_func(conn, **kwargs):
    return ActionResponse(Action.REQLLM, result, None)
```

# Pitfalls
- `config.yaml` contains secrets (API keys) — never commit `data/.config.yaml`
- `selected_module` in config.yaml controls which provider is active
- `read_config_from_api: true` makes server pull config from manager-api, overriding local files
