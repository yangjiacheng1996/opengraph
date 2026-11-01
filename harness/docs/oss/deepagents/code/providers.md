> ## Documentation Index
> Fetch the complete documentation index at: https://docs.langchain.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Model providers

> Configure any LangChain-compatible model provider for Deep Agents Code

Deep Agents Code supports any [chat model provider compatible with LangChain](https://docs.langchain.com/oss/python/integrations/chat.md), unlocking use for virtually any LLM that supports tool calling. Any service that exposes an OpenAI-compatible or Anthropic-compatible API also works out of the box—see [Compatible APIs](https://docs.langchain.com/oss/deepagents/code/config-file.md#compatible-apis).

## Quickstart

Deep Agents Code integrates automatically with the [following model providers](#provider-reference): no extra configuration needed beyond installing the relevant provider package.

1. **Install provider packages**

   Each model provider requires its corresponding LangChain integration package. These ship as optional extras to keep the application lightweight. OpenAI, Anthropic, and Gemini are included by default. Install any other extra from within a session with `/install`, or from the shell with `dcode --install`:

   ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /install groq
   ```

   ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   dcode --install groq
   ```

   Run `/install` with no argument to list the valid extras. To preinstall extras during the initial CLI install, set `DEEPAGENTS_CODE_EXTRAS`:

   ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   curl -LsSf https://langch.in/dcode | DEEPAGENTS_CODE_EXTRAS="baseten,groq" bash
   ```

2. **Set credentials**

   Add an API key for your provider with the [`/auth`](https://docs.langchain.com/oss/deepagents/code/credentials#use-%2Fauth-recommended) credential manager:

   ```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /auth
   ```

   `/auth` shows a list of available providers and stores credentials for reuse across sessions.

   For non-interactive runs, CI/CD, or anywhere a TUI is not available, store the same key from the shell with [`dcode auth set`](https://docs.langchain.com/oss/deepagents/code/credentials#manage-credentials-from-the-shell-dcode-auth) or set the provider's environment variable instead. See [Provider credentials](https://docs.langchain.com/oss/deepagents/code/credentials.md) for the full key resolution order, the [`DEEPAGENTS_CODE_` prefix](https://docs.langchain.com/oss/deepagents/code/configuration#deepagents_code_-prefix) for scoping a key to Deep Agents Code, and the [Provider reference](#provider-reference) for each provider's environment variable.

   To configure model parameters, see [Model parameters](#model-parameters).

## Provider reference

Using a provider not listed here? See [Arbitrary providers](https://docs.langchain.com/oss/deepagents/code/config-file.md#arbitrary-providers): any LangChain-compatible provider can be used in Deep Agents Code with additional setup.

| Provider | Package | Credential env var | Model profiles |
| - | - | - | - |
| OpenAI | [`langchain-openai`](https://docs.langchain.com/oss/python/integrations/chat/openai) | `OPENAI_API_KEY` | ✅ |
| OpenAI (Codex) | [`langchain-openai`](https://docs.langchain.com/oss/python/integrations/chat/openai) | None; [sign in with ChatGPT](#sign-in-with-chatgpt) | ✅ |
| Azure OpenAI | [`langchain-openai`](https://docs.langchain.com/oss/python/integrations/chat/azure_chat_openai) | `AZURE_OPENAI_API_KEY` | ✅ |
| Anthropic | [`langchain-anthropic`](https://docs.langchain.com/oss/python/integrations/chat/anthropic) | `ANTHROPIC_API_KEY` | ✅ |
| Google Gemini API | [`langchain-google-genai`](https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai) | `GOOGLE_API_KEY` | ✅ |
| Gemini Enterprise Agent Platform | [`langchain-google-genai`](https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai#credentials) | `GOOGLE_CLOUD_PROJECT` | ✅ |
| Gemini Enterprise Agent Platform (Anthropic) | [`langchain-google-vertexai`](https://docs.langchain.com/oss/python/integrations/chat/google_anthropic_vertex) | `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION` | ✅ |
| Baseten | [`langchain-baseten`](https://github.com/basetenlabs/langchain-baseten) | `BASETEN_API_KEY` | ✅ |
| AWS Bedrock | [`langchain-aws`](https://docs.langchain.com/oss/python/integrations/chat/bedrock) | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` | ✅ |
| AWS Bedrock Converse | [`langchain-aws`](https://docs.langchain.com/oss/python/integrations/chat/bedrock) | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` | ✅ |
| Hugging Face | [`langchain-huggingface`](https://docs.langchain.com/oss/python/integrations/chat/huggingface) | `HUGGINGFACEHUB_API_TOKEN` | ✅ |
| Ollama | [`langchain-ollama`](https://docs.langchain.com/oss/python/integrations/chat/ollama) | `OLLAMA_API_KEY` (cloud only; optional) | ❌ |
| Groq | [`langchain-groq`](https://docs.langchain.com/oss/python/integrations/chat/groq) | `GROQ_API_KEY` | ✅ |
| Cohere | [`langchain-cohere`](https://docs.langchain.com/oss/python/integrations/chat/cohere) | `COHERE_API_KEY` | ❌ |
| Fireworks | [`langchain-fireworks`](https://docs.langchain.com/oss/python/integrations/chat/fireworks) | `FIREWORKS_API_KEY` | ✅ |
| Together | [`langchain-together`](https://docs.langchain.com/oss/python/integrations/chat/together) | `TOGETHER_API_KEY` | ❌ |
| Meta | [`langchain-meta`](https://github.com/langchain-ai/langchain-meta) | `MODEL_API_KEY` | ✅ |
| Mistral AI | [`langchain-mistralai`](https://docs.langchain.com/oss/python/integrations/chat/mistralai) | `MISTRAL_API_KEY` | ✅ |
| DeepSeek | [`langchain-deepseek`](https://docs.langchain.com/oss/python/integrations/chat/deepseek) | `DEEPSEEK_API_KEY` | ✅ |
| IBM (watsonx.ai) | [`langchain-ibm`](https://docs.langchain.com/oss/python/integrations/chat/ibm_watsonx) | `WATSONX_APIKEY` | ❌ |
| Nvidia | [`langchain-nvidia-ai-endpoints`](https://docs.langchain.com/oss/python/integrations/chat/nvidia_ai_endpoints) | `NVIDIA_API_KEY` | ✅ |
| xAI | [`langchain-xai`](https://docs.langchain.com/oss/python/integrations/chat/xai) | `XAI_API_KEY` | ✅ |
| Perplexity | [`langchain-perplexity`](https://docs.langchain.com/oss/python/integrations/chat/perplexity) | `PERPLEXITY_API_KEY` (or `PPLX_API_KEY`) | ✅ |
| OpenRouter | [`langchain-openrouter`](https://docs.langchain.com/oss/python/integrations/chat/openrouter) | `OPENROUTER_API_KEY` | ✅ |
| LiteLLM | [`langchain-litellm`](https://docs.langchain.com/oss/python/integrations/chat/litellm) | Per-provider (see [docs](https://docs.litellm.ai/)) | ❌ |

> [accordion] Use Nemotron 3.5 Super VL on NVIDIA Build
>
> Install the NVIDIA provider package, then save your [NVIDIA Build API key](https://docs.langchain.com/oss/python/integrations/chat/nvidia_ai_endpoints#access-the-nvidia-api-catalog) with `/auth`:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> dcode --install nvidia
> dcode
> ```
>
> In dcode, run `/auth` and select NVIDIA. If the initial model selector is open, press `Esc` first. Then select Nemotron 3.5 Super VL from the model switcher.
>
> ```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> /model --default nvidia:nvidia/nemotron-3.5-super-vl-120b-a12b
> ```
>
> The NVIDIA provider uses the public Build endpoint by default. Check [NVIDIA Build](https://build.nvidia.com/) for model availability. For a configured private endpoint, set `NVIDIA_BASE_URL` or pair its URL with the credential through `dcode auth set nvidia --base-url URL`.

> [accordion] Configure Anthropic models on Gemini Enterprise Agent Platform
>
> The `google_anthropic_vertex` provider runs Claude through Anthropic's Messages API on Gemini Enterprise Agent Platform. It uses Google Cloud Application Default Credentials (ADC) instead of an Anthropic API key.
>
> To use the provider:
>
> 1. Install the Gemini Enterprise Agent Platform extra:
>
>    ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    /install vertex
>    ```
>
>    ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    dcode --install vertex
>    ```
>
> 2. [Enable a Claude model in your Google Cloud project](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/partner-models/claude), then configure ADC:
>
>    ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    gcloud auth application-default login
>    ```
>
> 3. Set your Google Cloud project and location:
>
>    ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    export GOOGLE_CLOUD_PROJECT="your-project-id"
>    export GOOGLE_CLOUD_LOCATION="global"
>    ```
>
>    You can use `DEEPAGENTS_CODE_GOOGLE_CLOUD_PROJECT` and `DEEPAGENTS_CODE_GOOGLE_CLOUD_LOCATION` to scope these values to Deep Agents Code.
>
> 4. Select a Claude model with the `google_anthropic_vertex` provider:
>
>    ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    /model google_anthropic_vertex:claude-sonnet-4-6
>    ```
>
>    ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
>    dcode --model google_anthropic_vertex:claude-sonnet-4-6
>    ```
>
> Use `google_vertexai` for Google models. Claude models use Anthropic's Messages API and must use `google_anthropic_vertex` instead.

> [!TIP]
>
> You can scope any credential to Deep Agents Code by adding a `DEEPAGENTS_CODE_` prefix. For example, `DEEPAGENTS_CODE_OPENAI_API_KEY` takes priority over `OPENAI_API_KEY` within Deep Agents Code without affecting other tools. See [`DEEPAGENTS_CODE_` prefix](https://docs.langchain.com/oss/deepagents/code/configuration#deepagents_code_-prefix) for details.

> [!TIP]
>
> [Model profiles](https://docs.langchain.com/oss/python/langchain/models.md#model-profiles) provide model metadata used by the interactive `/model` switcher. If a model is missing from the switcher, pass the model name directly or add it via `config.toml`.

### Sign in with ChatGPT

The `openai_codex` provider lets you use OpenAI's Codex models with your paid **ChatGPT** subscription instead of an `OPENAI_API_KEY`. You sign in with your ChatGPT account, and it shows up as its own provider in both `/auth` and the `/model` switcher, separate from the API-key-based `openai` provider.

**1. Start the sign-in**

Run `/auth` in any session and select **`openai_codex`**. Because ChatGPT signs you in through your browser, this starts a browser sign-in instead of asking for an API key.

**2. Authorize in your browser**

Deep Agents Code opens your browser to the ChatGPT sign-in page. If it cannot open a browser (for example, over SSH), it also shows the sign-in URL on screen so you can copy it to a browser on another device.

**3. Select a Codex model**

Once signed in, the Codex models appear in the `/model` switcher under the `openai_codex` provider. Switch to one directly with its spec:

```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/model openai_codex:gpt-5.5
```

Your sign-in persists across sessions. To check your status or sign out, run `/auth`, select `openai_codex`, and choose to re-authenticate or sign out.

> [!NOTE]
>
> `openai_codex` is separate from `openai`. To use OpenAI models with a standard API key instead, use the regular `openai` provider (e.g. `/model openai:gpt-5.5`).

> [!NOTE]
>
> Some provider-specific account types or key scopes may not work for API access. If a provider appears configured in `/auth` but requests still fail, verify that the account plan and API-key permissions match the provider's API requirements.

### Model routers and proxies

Model routers like [OpenRouter](https://openrouter.ai/) and [LiteLLM](https://docs.litellm.ai/) provide access to models from multiple providers through a single endpoint.

Use the dedicated integration packages for these services:

| Router | Package | Config |
| - | - | - |
| OpenRouter | [`langchain-openrouter`](https://docs.langchain.com/oss/python/integrations/chat/openrouter) | `openrouter:<model>` (built-in, see [Provider reference](#provider-reference)) |
| LiteLLM | [`langchain-litellm`](https://docs.langchain.com/oss/python/integrations/chat/litellm) | `litellm:<model>` (built-in, see [Provider reference](#provider-reference)) |

**OpenRouter** is a built-in provider—install the extra and use it directly:

```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/install openrouter
```

```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode --install openrouter
```

**LiteLLM** is also a built-in provider:

```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/install litellm
```

```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode --install litellm
```

## Switch models

To switch models in Deep Agents Code, either:

1. **Use the interactive model switcher** with the `/model` command.

   > [!NOTE]
   >
   > Not all models appear here. If yours is missing, pass the model name directly (e.g. `/model gpt-5.5`) or add it to `config.toml`.
2. **Specify a model name directly** as an argument, e.g. `/model gpt-5.5`. You can use any model supported by the chosen provider, regardless of whether it appears in the list from option 1. The model name will be passed to the API request.
3. **Specify the model at launch** via `--model`, e.g.

   ```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   dcode --model openai:gpt-5.5
   ```

> [accordion] Model resolution order
>
> When Deep Agents Code launches, it resolves which model to use in the following order:
>
> 1. **`--model` flag** always wins when provided.
> 2. **`[models].default`** in `~/.deepagents/config.toml`—the user's intentional long-term preference.
> 3. **`[models].recent`** in `~/.deepagents/config.toml`—the last model switched to via `/model`. Written automatically; never overwrites `[models].default`.
> 4. **Environment auto-detection**: falls back to the first available startup credential, checked in order: `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `GOOGLE_API_KEY`, `GOOGLE_CLOUD_PROJECT` (Gemini Enterprise Agent Platform).
>
> This startup fallback intentionally checks only those four credentials. Other supported providers (for example, Groq) are still available via `--model`, `/model`, and saved defaults (`[models].default` / `[models].recent`).

### Which models appear in the switcher

The `/model` selector dynamically builds its list from installed provider packages. Expand below for the full criteria and troubleshooting.

> [accordion] How the switcher builds its model list
>
> The interactive `/model` selector builds its list from installed provider packages and models configured in `config.toml`.
>
> A model appears when:
>
> 1. The provider package is installed.
> 2. The model is available from the provider package, a local provider, or your `config.toml`.
> 3. The model profile does not mark text input or output as unsupported.
>
> If a model is missing, use `/model <provider>:<model>` directly or add it to [`[models.providers.<name>].models`](https://docs.langchain.com/oss/deepagents/code/config-file#adding-models-to-the-interactive-switcher).
>
> > [!TIP]
> >
> > Credential status does **not** affect whether a model is listed. You can still select a model with missing credentials. The provider reports an authentication error at request time.

### Open weights models

If you want to use an open weights model, there are two common paths depending on whether you prefer local or cloud-hosted inference.

**Local inference with Ollama** is the easiest way to get started for free, with no API key required:

1. [Install Ollama](https://ollama.com/) and pull a model, for example:

   ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   ollama pull qwen3:4b
   ```

2. Install the Ollama extra:

   ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /install ollama
   ```

   ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   dcode --install ollama
   ```

3. Select the model:

   ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /model
   ```

   ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   dcode --model ollama:qwen3:4b
   ```

   Use the interactive switcher, or pass the model directly with `/model ollama:qwen3:4b`.

**Cloud-hosted open weights via Groq** gives you fast inference without running anything locally:

1. Get a free API key at [console.groq.com](https://console.groq.com/).

2. Install the Groq extra:

   ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /install groq
   ```

   ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   dcode --install groq
   ```

3. Select a model:

   ```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /model
   ```

   ```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   GROQ_API_KEY="your-api-key" dcode --model groq:openai/gpt-oss-120b
   ```

   Use the interactive switcher, or pass the model directly with `/model groq:openai/gpt-oss-120b`.

**Fireworks** is another popular cloud provider for open weights models:

```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/install fireworks
/model
```

```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode --install fireworks
FIREWORKS_API_KEY="your-api-key" dcode --model fireworks:accounts/fireworks/models/deepseek-v4-pro
```

Use the interactive switcher, or pass the model directly with `/model fireworks:accounts/fireworks/models/deepseek-v4-pro`.

**Baseten** is another cloud provider for open weights models:

```txt In session theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
/install baseten
/model
```

```bash Shell theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode --install baseten
BASETEN_API_KEY="your-api-key" dcode --model baseten:moonshotai/Kimi-K2.7-Code
```

Use the interactive switcher, or pass the model directly with `/model baseten:moonshotai/Kimi-K2.7-Code`.

> [!TIP]
>
> If you want a provider preinstalled at the same time as the CLI itself, use `DEEPAGENTS_CODE_EXTRAS` during the initial install:
>
> ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
> curl -LsSf https://langch.in/dcode | DEEPAGENTS_CODE_EXTRAS="fireworks" bash
> ```
>
> You can combine multiple providers: `DEEPAGENTS_CODE_EXTRAS="groq,fireworks,ollama"`. If Deep Agents Code is already installed, use `/install <extra>` in a session or `dcode --install <extra>` from the shell instead.

**Together**, **OpenRouter**, and **Hugging Face** (`langchain-huggingface`) are other options for cloud-hosted open weights. See the [Provider reference](#provider-reference) for credentials and package names.

### Set a default model

You can set a persistent default model that applies to all future CLI launches:

* **Via model selector:** Open `/model`, navigate to the desired model, and press `Ctrl+S` to pin it as the default. Pressing `Ctrl+S` again on the current default clears it.
* **Via command:** `/model --default provider:model` (e.g., `/model --default anthropic:claude-opus-4-8`)
* **Via config file:** Set `[models].default` in `~/.deepagents/config.toml` (see [Configuration](https://docs.langchain.com/oss/deepagents/code/configuration)).
* **From the shell:**

  ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  dcode --default-model anthropic:claude-opus-4-8
  ```

To view the current default:

```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
dcode --default-model
```

To clear the default:

* **From the shell:**

  ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
  dcode --clear-default-model
  ```

* **Via command:** `/model --default --clear`

* **Via model selector:** Press `Ctrl+S` on the currently pinned default model.

Without a default, Deep Agents Code uses the most recently used model.

### Model parameters

Pass extra constructor kwargs to the model—sampling controls, reasoning/thinking budgets, context window sizes, request timeouts, and anything else the underlying chat-model class accepts. Three places to set them, in priority order (highest first):

1. **One-off at launch with `--model-params`.** JSON string, session-only:

   ```bash theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   # OpenAI reasoning effort
   dcode --model openai:gpt-5.5 --model-params '{"reasoning": {"effort": "high"}}'

   # Anthropic extended thinking
   dcode --model anthropic:claude-opus-4-8 --model-params '{"thinking": {"type": "enabled", "budget_tokens": 10000}, "max_tokens": 16000}'
   ```

2. **Mid-session via `/model --model-params`.** Same JSON syntax—swaps params (and optionally the model) without restarting:

   ```txt theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   /model --model-params '{"temperature": 0.7}' anthropic:claude-opus-4-8
   /model --model-params '{"num_ctx": 16384}'           # opens selector, applies params to choice
   ```

3. **Persistent in `config.toml`.** Provider-level defaults (with optional per-model sub-tables) that apply on every launch:

   ```toml theme={"theme":{"light":"catppuccin-latte","dark":"catppuccin-mocha"}}
   [models.providers.anthropic.params]
   thinking = { type = "enabled", budget_tokens = 10000 }
   max_tokens = 16000

   [models.providers.openai.params]
   reasoning = { effort = "high", summary = "auto" }
   output_version = "responses/v1"

   [models.providers.ollama.params]
   num_ctx = 16384
   temperature = 0

   # Per-model override—wins over provider-level keys
   [models.providers.ollama.params."qwen3:4b"]
   temperature = 0.5
   ```

CLI flags override config-file `params` and are session-only (mid-session changes are not persisted). Per-model sub-tables in `config.toml` override provider-level keys (shallow merge—see [Model constructor params](https://docs.langchain.com/oss/deepagents/code/config-file.md#model-constructor-params) for full semantics). `--model-params` cannot be combined with `--default`.

For retry counts, prefer `--max-retries` or the top-level [`[retries]` config](https://docs.langchain.com/oss/deepagents/code/config-file#retries).

> [!TIP]
>
> Any kwarg accepted by the underlying chat-model constructor is valid. Refer to the provider's reference docs for the full list—e.g. [`ChatAnthropic`](https://reference.langchain.com/python/langchain-anthropic/langchain_anthropic/chat_models/ChatAnthropic), [`ChatOpenAI`](https://reference.langchain.com/python/langchain-openai/langchain_openai/chat_models/base/ChatOpenAI), [`ChatOllama`](https://reference.langchain.com/python/langchain-ollama/langchain_ollama/chat_models/ChatOllama). Unknown kwargs are forwarded to the upstream API request, so newly released parameters work without a CLI update.

> [!NOTE]
>
> Do not put credentials (`api_key`) in `params`—use [`api_key_env`](https://docs.langchain.com/oss/deepagents/code/config-file#provider-configuration) to point at an environment variable instead.

To override fields on the model's runtime *profile* (`max_input_tokens`, `tool_calling`, capability flags)—distinct from constructor params—see [Profile overrides](https://docs.langchain.com/oss/deepagents/code/config-file.md#profile-overrides-advanced).

## Advanced configuration

For detailed configuration of provider params, profile overrides, custom base URLs, compatible APIs, arbitrary providers, and lifecycle hooks, see [Config file](https://docs.langchain.com/oss/deepagents/code/config-file.md) and [Hooks](https://docs.langchain.com/oss/deepagents/code/hooks.md).

***

[Connect these docs](https://docs.langchain.com/use-these-docs.md) to your agent of choice via MCP for real-time answers.

[Edit this page on GitHub](https://github.com/langchain-ai/docs/edit/main/src/oss/deepagents/code/providers.mdx) or [file an issue](https://github.com/langchain-ai/docs/issues/new/choose).
