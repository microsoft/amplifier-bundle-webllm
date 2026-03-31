# amplifier-bundle-webllm

WebLLM provider for Amplifier - local browser-based LLM inference via WebGPU.

## Overview

Run LLMs entirely in the browser using WebGPU acceleration. No server required, no API keys, complete privacy.

**Key Benefits:**
- **Offline-capable** - Works without internet after initial model download
- **Private** - Data never leaves the user's device
- **Free** - No per-request API costs
- **Fast** - Hardware-accelerated via WebGPU

## Quick Start

### 1. Compose with Browser Bundle

```yaml
# my-app.yaml
includes:
  - bundle: git+https://github.com/microsoft/amplifier-bundle-browser@main
  - bundle: git+https://github.com/microsoft/amplifier-bundle-webllm@main
```

### 2. Initialize in JavaScript

```javascript
import { CreateMLCEngine } from '@mlc-ai/web-llm';

// Load model (downloads on first use, cached afterward)
const engine = await CreateMLCEngine('Phi-3.5-mini-instruct-q4f16_1-MLC', {
    initProgressCallback: (p) => console.log(`Loading: ${Math.round(p.progress * 100)}%`)
});

// Create bridge for Amplifier
pyodide.globals.set('js_llm_complete', async (reqJson) => {
    const req = JSON.parse(reqJson);
    const res = await engine.chat.completions.create(req);
    return JSON.stringify(res);
});

// Register with provider
await pyodide.runPythonAsync(`
    from amplifier_module_provider_webllm import set_llm_bridge
    import js
    set_llm_bridge(js.js_llm_complete)
`);
```

## Requirements

### Hardware

| Model Size | Minimum VRAM | Recommended |
|------------|--------------|-------------|
| 1-3B params | 4GB | 6GB+ |
| 7-8B params | 8GB | 12GB+ |

### Browser

| Browser | WebGPU Support |
|---------|---------------|
| Chrome 113+ | ✅ Full |
| Edge 113+ | ✅ Full |
| Safari 18+ | ✅ Full |
| Firefox | ⚠️ Behind flag |

## Supported Models

| Model | Size | VRAM | Quality | Speed |
|-------|------|------|---------|-------|
| **Phi-3.5-mini-instruct-q4f16_1-MLC** | 2.4GB | 4GB | Good | Fast |
| **Llama-3.2-3B-Instruct-q4f16_1-MLC** | 2.1GB | 4GB | Good | Fast |
| **Llama-3.1-8B-Instruct-q4f16_1-MLC** | 4.5GB | 8GB | Better | Medium |
| **Qwen2.5-7B-Instruct-q4f16_1-MLC** | 4.2GB | 8GB | Better | Medium |

See [MLC-LLM](https://mlc.ai/mlc-llm/docs/) for full model list.

## What's Included

### Provider

| Module | Description |
|--------|-------------|
| `provider-webllm` | WebLLM provider implementing Amplifier's Provider protocol |

### Context

- `webllm-guide.md` - Model selection, setup, optimization tips

## Directory Structure

```
amplifier-bundle-webllm/
├── bundle.yaml                 # Main entry point
├── behaviors/
│   └── webllm.yaml             # WebLLM behavior
├── providers/
│   └── webllm.yaml             # Default provider config
├── context/
│   └── webllm-guide.md         # WebLLM guidance
└── modules/
    └── provider-webllm/        # Provider implementation
```

## Configuration

```yaml
providers:
  - module: provider-webllm
    source: webllm:modules/provider-webllm
    config:
      default_model: "Phi-3.5-mini-instruct-q4f16_1-MLC"
      temperature: 0.7
      max_tokens: 1024
```

## Model Loading

### First Load

Models are downloaded from MLC-AI CDN on first use:
- **Phi-3.5-mini**: ~2.4GB download
- **Time**: 1-5 minutes depending on connection

### Cached Loads

After first download, models load from browser cache:
- **Time**: 5-15 seconds
- **Works offline**: Yes

## Performance Tips

1. **Use q4f16 quantization** - Best balance of size/quality/speed
2. **Close GPU-intensive tabs** - Free up VRAM
3. **Warm up the model** - First inference is slower
4. **Match model to hardware** - Don't load 8B on 4GB VRAM

## Architecture

```
┌─────────────────────────────────────────┐
│  JavaScript                             │
│  ├── WebLLM Engine                      │
│  │   └── WebGPU inference               │
│  └── Bridge function                    │
└─────────────────┬───────────────────────┘
                  │ JSON (request/response)
                  ▼
┌─────────────────────────────────────────┐
│  Python (Pyodide)                       │
│  ├── WebLLMProvider                     │
│  └── Amplifier session                  │
└─────────────────────────────────────────┘
```

## Error Handling

| Error | Solution |
|-------|----------|
| "WebGPU not supported" | Use Chrome 113+ or Edge 113+ |
| "Out of memory" | Use smaller model (Phi-3.5-mini) |
| "Bridge not initialized" | Call `set_llm_bridge()` first |

## License

MIT

## Related

- [amplifier-bundle-browser](https://github.com/microsoft/amplifier-bundle-browser) - Browser runtime
- [WebLLM](https://webllm.mlc.ai/) - Browser LLM engine
- [MLC-LLM](https://mlc.ai/mlc-llm/) - Model compilation

## Contributing

> [!NOTE]
> This project is not currently accepting external contributions, but we're actively working toward opening this up. We value community input and look forward to collaborating in the future. For now, feel free to fork and experiment!

Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit [Contributor License Agreements](https://cla.opensource.microsoft.com).

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
