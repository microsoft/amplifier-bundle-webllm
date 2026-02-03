# WebLLM Capabilities

You have access to **WebLLM** for browser-based local LLM inference via WebGPU.

## What WebLLM Provides

- **Fully offline AI** - Works without internet after initial model download
- **Complete privacy** - Data never leaves the user's device
- **No API costs** - Local inference using device GPU
- **WebGPU acceleration** - Hardware-accelerated performance

## Architecture (Critical)

WebLLM in Amplifier is a **PROVIDER**, not standalone JavaScript:

```
Browser App → Pyodide → amplifier-core → WebLLM Provider → WebLLM Engine
```

Raw JavaScript WebLLM is ONLY for explicit requests using "pure JavaScript", "no Python", "raw WebLLM".

## Browser Requirements

| Browser | Support |
|---------|---------|
| Chrome 113+ | ✅ Full |
| Edge 113+ | ✅ Full |
| Safari 18+ | ✅ Full |
| Firefox | ⚠️ Behind flag |

## Common Models

| Model | VRAM | Best For |
|-------|------|----------|
| Phi-3.5-mini-instruct-q4f16_1-MLC | 4GB | General use, low-end hardware |
| Llama-3.2-3B-Instruct-q4f16_1-MLC | 4GB | Fast responses |
| Qwen2.5-7B-Instruct-q4f16_1-MLC | 8GB | Multilingual, quality |

## When to Delegate

For detailed guidance on model selection, error handling, architecture patterns, or WebGPU troubleshooting, delegate to `webllm:webllm-expert`.
