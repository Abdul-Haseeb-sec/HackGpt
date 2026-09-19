<!-- HackGPT document -->
# 🚀 HackGPT Enterprise Release Notes — Version 2026.09.19

> [!IMPORTANT]
> ### 💖 Critical Notice: Support, Donations & Sponsorship Required
> HackGPT Enterprise is an independent open-source cybersecurity and AI penetration testing research project. To continuously maintain the platform, support multi-provider frontier AI integrations, and sustain security research infrastructure, **we require funds, donations, and sponsorships to maintain this project**.
>
> **Please consider donating and sponsoring HackGPT development!**
> - **GitHub Sponsors (Active)**: [Sponsor @yashab-cyber on GitHub Sponsors](https://github.com/sponsors/yashab-cyber)
> - **Cryptocurrency Transfers**: For crypto donations (Solana, Bitcoin, Ethereum, USDT, etc.), please email: **yashabalam707@gmail.com**
> - **Sponsorships & Inquiries**: Contact creator at: **yashabalam707@gmail.com**
> - **Full Guidelines & Tier Perks**: Please visit [DONATE.md](DONATE.md).

---

We are excited to announce the release of **HackGPT Enterprise Version 2026.09.19**! This major release delivers dynamic multi-provider AI model auto-fetching, frontier model catalog expansion (`gpt-astra`, `gpt-6-astra`, `claude-3.7-sonnet`, `gemini-2.0`, `o1`, `deepseek-r1-zero`), custom routing gateways (including OpenRouter overrides and dedicated 9B task dispatchers), and comprehensive stability and bug fixes across the platform.

---

## 🌟 What's New in Version 2026.09.19

### 1. Dynamic Multi-Provider Remote Model Auto-Fetching
* **Live Discovery Across 9+ AI Providers**: Integrated dynamic query capabilities (`fetch_remote_models()`) allowing HackGPT to automatically query live provider endpoints and discover new models at runtime:
  * **OpenAI**: Dynamic discovery via `client.models.list()` or `/v1/models` endpoint for instant access to new models.
  * **Anthropic (Claude)**: Direct querying of `https://api.anthropic.com/v1/models` with fallback to latest Claude frontier catalog (`claude-3-7-sonnet`, `claude-3-5-sonnet`, `claude-3-opus`).
  * **Google Gemini**: Dynamic discovery via Google Generative Language API (`/models?key=...`), pulling model limits, context windows, and multimodal capabilities.
  * **DeepSeek**: Real-time querying of DeepSeek models (`deepseek-chat`, `deepseek-reasoner` R1, and `deepseek-r1-zero`).
  * **GLM (Zhipu AI)**: Real-time query support for BigModel GLM endpoints and frontier models (`glm-4-plus`, `glm-4-air`, `glm-4-flash`, `glm-4-long`, `codegeex-4`, `glm-zero-preview`).
  * **Local Ollama**: Automatic discovery of locally pulled models via `/api/tags`.
  * **OpenRouter Aggregator**: Live model discovery across hundreds of upstream endpoints via `/models`.
  * **9B Router & Custom Gateways**: Automated inspection of custom router `/models` endpoints.
* **Dynamic Model Catalog & Normalizer (`ai_engine/model_registry.py`)**:
  * `DYNAMIC_MODEL_CATALOG`: Real-time runtime store for discovered models.
  * `fetch_all_provider_models()`: Automated discovery across all or specified providers with caching and fallback.
  * `normalize_provider()`: Robust provider alias resolution supporting flexible strings (`'openai'`, `'claude'`, `'anthropic'`, `'gemini'`, `'google'`, `'deepseek'`, `'glm'`, `'zhipu'`, `'openrouter'`, `'9brouter'`, `'custom'`).

### 2. Frontier AI Model Catalog Expansion
* Added official support, token windows, and tool-calling parameters for:
  * **GPT Astra** (`gpt-astra`) & **GPT-6 Astra** (`gpt-6-astra`)
  * **Claude 3.7 Sonnet** (`claude-3.7-sonnet`)
  * **Google Gemini 2.0 Flash & Pro** (`gemini-2.0-flash`, `gemini-2.0-pro`)
  * **OpenAI o1 & o1-mini** (`o1`, `o1-mini`)
  * **DeepSeek R1 Zero** (`deepseek-r1-zero`)

### 3. Custom Routers & Intelligent Task Dispatching
* **9B Router (`9brouter/*`)**: Specialized client (`NineBRouterProvider`) for 9B parameter task and exploitation routers (e.g. Qwen 2.5 9B, Gemma 2 9B, Llama 3.1 9B) with OpenAI SDK integration and raw HTTP failover.
* **OpenRouter Custom Endpoints (`OPENROUTER_BASE_URL`)**: Configurable base URL overrides for private OpenRouter proxies and enterprise gateways.
* **Custom Router Gateway (`custom_router/*`, `custom/*`)**: Generic client (`CustomRouterProvider`) allowing routing to any internal OpenAI-compatible reverse proxy, vLLM, LiteLLM, or Portkey gateway.
* **Dynamic Routing Prefixes**: Seamless on-the-fly resolution for models prefixed with `openrouter/`, `9brouter/`, `custom_router/`, or `custom/`.

### 4. Enterprise CLI & Interactive Management
* Added `--model`, `--provider`, and `--custom-route` CLI options.
* Added `--fetch-models` to query and register live models from configured providers on startup.
* Added `--list-models` to print full formatted tables of cataloged and discovered models.
* Integrated automated model discovery into interactive configuration menu option **`11`** (`configure_ai_engine`).

### 5. Platform Hardening & Bug Fixes
* Fixed database session lifecycle management in PostgreSQL/SQLite adapters.
* Resolved task concurrency handling in Celery/ParallelProcessor pipelines.
* Corrected IOC pattern extraction for edge-case RFC IPv6 and domain representations in the SOC analysis engine.
* Added safe error handling and resilient fallbacks during network timeouts and offline execution.

---

## 🛠️ Quick Start with Version 2026.09.19

1. **Auto-Fetch Models from Providers**:
   ```bash
   python advance_hackgpt.py --fetch-models
   ```

2. **List All Available Models**:
   ```bash
   python advance_hackgpt.py --list-models
   ```

3. **Launch with Specific Model & Provider**:
   ```bash
   python advance_hackgpt.py --model gpt-astra --provider openai
   python advance_hackgpt.py --model openrouter/anthropic/claude-3.7-sonnet
   python advance_hackgpt.py --model 9brouter/agent-router --custom-route http://localhost:8000/v1
   ```

4. **Verify Installation & Test Suite**:
   ```bash
   pytest tests/unit/ -v --tb=short
   python test_installation.py
   ```
