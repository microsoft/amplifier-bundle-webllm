---
bundle:
  name: webllm
  version: 1.0.0
  description: Browser-based local LLM inference via WebGPU with Amplifier integration

includes:
  - bundle: git+https://github.com/microsoft/amplifier-foundation@main
  - bundle: webllm:behaviors/webllm
---

# WebLLM Bundle

**Run local LLMs in the browser with WebGPU acceleration.**

Build browser-based AI applications using WebLLM for inference and Amplifier for session management.

## What This Bundle Provides

### From webruntime (included via behavior)
- **webruntime-developer** agent for building browser Amplifier apps
- Pyodide integration patterns
- Autonomous Playwright testing

### WebLLM-Specific
- **webllm-expert** agent for model selection and architecture guidance
- WebLLM model selection guidance
- WebGPU optimization patterns
- Browser compatibility information

## Quick Start

```
"Build me a WebLLM chat app as a single HTML file"
"Create an offline AI assistant with Phi-3.5"
"Make a browser-based coding helper"
```

## Supported Models

| Model | Size | VRAM | Best For |
|-------|------|------|----------|
| Phi-3.5-mini-instruct-q4f16_1-MLC | 2.4GB | 4GB | General use |
| Llama-3.2-3B-Instruct-q4f16_1-MLC | 2.1GB | 4GB | Fast responses |
| Qwen2.5-7B-Instruct-q4f16_1-MLC | 4.2GB | 8GB | Multilingual |

For detailed model selection, architecture guidance, or troubleshooting, delegate to `webllm:webllm-expert`.
