# PromptForge

PromptForge is a browser-based AI red teaming workbench for testing LLM and agent behavior against realistic prompt-injection, tool-abuse, and excessive-agency scenarios. It gives you a structured place to run attack templates, compare providers, inspect prompts, and generate defensive analysis reports.

The project is built with React, TypeScript, and Vite. It runs locally and calls provider APIs directly from your browser.

## Features

- OWASP-style LLM attack templates for prompt injection, insecure output handling, sensitive-data disclosure, excessive agency, insecure plugin design, model theft, and related agent risks.
- Multi-provider testing with Gemini, OpenAI, Claude/Anthropic, and local Ollama models.
- In-app API key entry for Gemini, OpenAI, and Claude. Keys are saved only in browser localStorage.
- Adversarial Mode, where a Gemini-powered red-team assistant suggests the next attack prompt for templates that define a goal.
- Defense Analysis, where a Gemini-powered AISecOps analyst summarizes conversation risk and recommends mitigations.
- Prompt debugger with injection keyword highlighting.
- Custom attack template manager saved in browser storage.
- Simulated agent tools for testing tool-use behavior and agency boundaries.
- Response caching for repeatable experiments.

## Tech Stack

- React 19
- TypeScript
- Vite
- Google Generative AI SDK
- Direct browser `fetch` integrations for OpenAI, Anthropic, and Ollama

## Prerequisites

- Node.js 18 or newer
- npm
- At least one provider key if you want to test hosted models:
  - Gemini API key from Google AI Studio
  - OpenAI API key
  - Anthropic API key
- Optional: Ollama installed locally for local model testing

## Quick Start

```bash
npm install
npm run dev
```

Open the local URL printed by Vite, usually:

```text
http://localhost:3000
```

## API Keys

PromptForge expects API keys to be entered in the app.

Use the left-side Configuration panel:

1. Select a provider: Gemini, OpenAI, Claude, or Ollama.
2. Paste the API key or base URL in the API Credentials box.
3. Wait for the validation indicator.
4. Choose a model and start testing.

These values are stored in your browser localStorage under this app's origin. They are not written to project files.

## Supported Providers And Models

The dropdown includes current text/chat model IDs for:

- Gemini: `gemini-3.5-flash`, `gemini-3.1-pro-preview`, `gemini-3-flash-preview`, and stable Gemini 2.5 models.
- OpenAI: GPT-5.5, GPT-5.4, GPT-5, GPT-4.1, and GPT-4o family models.
- Claude: Claude Opus 4.7, Sonnet 4.6, Haiku 4.5, and recent 4.x/3.x fallbacks.
- Ollama: common local model names such as `qwen3.5`, `glm-5.1`, `mistral-medium-3.5`, `llama4`, `qwen3`, `gemma3`, `deepseek-r1`, and others.

Model availability can depend on your provider account, region, access tier, and local Ollama pulls. If a model returns a provider error, choose another model from the same provider or confirm access in the provider dashboard.

## Using Ollama

Install Ollama from:

```text
https://ollama.com/
```

Pull the models you want to use. Example:

```bash
ollama pull qwen3
ollama pull gemma3
ollama pull deepseek-r1
```

Set the Ollama Base URL in PromptForge:

```text
http://localhost:11434
```

If the browser cannot connect, configure Ollama CORS.

### Windows

Set a system environment variable:

```text
OLLAMA_ORIGINS=*
```

Restart Ollama, or restart your computer.

### macOS

```bash
launchctl setenv OLLAMA_ORIGINS "*"
```

Then restart Ollama.

### Linux systemd

```bash
sudo systemctl edit ollama.service
```

Add:

```ini
[Service]
Environment="OLLAMA_ORIGINS=*"
```

Then run:

```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

## Available Scripts

```bash
npm run dev
```

Starts the Vite development server.

```bash
npm run build
```

Builds the production bundle into `dist/`.

```bash
npm run preview
```

Serves the production build locally.

```bash
npm run lint
```

Runs TypeScript checks with `tsc --noEmit`.

## Project Structure

```text
.
├── App.tsx
├── constants.ts
├── index.html
├── index.tsx
├── types.ts
├── vite.config.ts
├── components/
├── services/
│   └── llmService.ts
├── package.json
└── README.md
```

## Security Notes

- This is a client-side research tool. API calls are made from the browser.
- Browser-stored keys are convenient for local use, but they are not appropriate for a shared or public deployment.
- Do not deploy this app publicly with direct user-supplied provider keys unless you understand the security and billing risks.
- Never commit real API keys, logs containing keys, or exported browser storage.
- Red-team templates can intentionally produce unsafe prompts or outputs. Use the tool only in authorized testing environments.

## Making A GitHub Release

Before pushing:

```bash
npm install
npm run lint
npm run build
```

Check that these generated or local-only folders are not committed:

- `node_modules/`
- `dist/`
- local logs

## Troubleshooting

### Gemini returns "model not found"

The selected model may not be available for your API version, account, or region. Try `gemini-3.5-flash` or `gemini-3-flash-preview`.

### Defense Analysis or Adversarial Mode fails

Those features require a Gemini key. Select Gemini in the Configuration panel and paste your Gemini API key in the API Credentials box.

### OpenAI or Claude key validates but generation fails

Some models require account access or have provider-specific parameter restrictions. Try a smaller or older model from the dropdown.

### Ollama connection fails

Confirm Ollama is running, the model is pulled, the base URL is correct, and `OLLAMA_ORIGINS=*` is configured if your browser blocks the request.

## Contributing

Pull requests are welcome. Keep changes focused, run the checks above, and avoid committing generated files or secrets.

