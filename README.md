# Claw Code — Azure OpenAI Edition

<p align="center">
  <strong>Claude Code harness with native Azure OpenAI support — built for enterprise and air-gapped environments</strong>
</p>

<p align="center">
  <a href="https://github.com/EugeneXL/claw-code-openai">Fork of ultraworkers/claw-code</a>
  ·
  <a href="./USAGE.md">Usage</a>
  ·
  <a href="./rust/README.md">Rust workspace</a>
  ·
  <a href="./PARITY.md">Parity</a>
  ·
  <a href="https://discord.gg/5TUQKKqFWd">UltraWorkers Discord</a>
</p>

<p align="center">
  <a href="https://star-history.com/#ultraworkers/claw-code&Date">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=ultraworkers/claw-code&type=Date&theme=dark" />
      <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=ultraworkers/claw-code&type=Date" />
      <img alt="Star history for claw-code" src="https://api.star-history.com/svg?repos=ultraworkers/claw-code&type=Date" width="600" />
    </picture>
  </a>
</p>

<p align="center">
  <img src="assets/claw-hero.jpeg" alt="Claw Code" width="300" />
</p>

---

## What is this fork?

This is the **Azure OpenAI edition** of the public Claude Code agent harness. It is forked from [ultraworkers/claw-code](https://github.com/ultraworkers/claw-code) and adds first-class Azure OpenAI support — covering GPT-4o, GPT-4o mini, o1, o3, and any Azure-hosted OpenAI-compatible model — plus input-token auto-compaction for long-running sessions.

If you need to run Claude Code-style CLI agent tooling **inside an Azure environment**, behind a **corporate firewall**, or with **enterprise compliance requirements**, this fork is for you.

---

## Provider support

| Provider | Status | Auth |
|----------|--------|------|
| Azure OpenAI | ✅ Native | `api-key` header |
| OpenAI | ✅ Compatible | Bearer token |
| Anthropic | ✅ Compatible | Bearer token |
| DashScope (Qwen) | ✅ Compatible | Bearer token |

---

## Azure OpenAI quick setup

**Option 1 — Full deployment URL**

```bash
export AZURE_OPENAI_BASE_URL="https://YOUR-RESOURCE.openai.azure.com/openai/deployments/YOUR-DEPLOYMENT?api-version=2024-10-21"
export AZURE_OPENAI_API_KEY="your-azure-key"

cd rust
./target/debug/claw --model "YOUR-DEPLOYMENT" prompt "say hello"
```

**Option 2 — Endpoint + deployment + api-version**

```bash
export AZURE_OPENAI_ENDPOINT="https://YOUR-RESOURCE.openai.azure.com"
export AZURE_OPENAI_DEPLOYMENT="YOUR-DEPLOYMENT"
export AZURE_OPENAI_API_VERSION="2024-10-21"
export AZURE_OPENAI_API_KEY="your-azure-key"

cd rust
./target/debug/claw --model "YOUR-DEPLOYMENT" prompt "say hello"
```

> [!TIP]
> The **deployment name** goes in `--model`. If your organization uses an API gateway in front of Azure, the same `endpoint + deployment + api-version + api-key` flow works.

---

## Input-token auto compact

Long-running sessions auto-compact before the next request is sent. Configure in `.claw.json` or `settings.json`:

```json
{
  "plugins": {
    "maxInputTokens": 240000,
    "maxOutputTokens": 8192
  }
}
```

`maxInputTokens` triggers automatic compaction of older conversation history. If compaction can't reduce context enough, `claw` fails early with a clear error instead of waiting for the provider to reject the request.

---

## Build from source

```bash
git clone https://github.com/EugeneXL/claw-code-openai
cd claw-code-openai/rust
cargo build --workspace

# Set your API key
export AZURE_OPENAI_API_KEY="your-azure-key"
export AZURE_OPENAI_BASE_URL="https://YOUR-RESOURCE.openai.azure.com/openai/deployments/YOUR-DEPLOYMENT?api-version=2024-10-21"

# Verify
./target/debug/claw doctor

# Run
./target/debug/claw --model "YOUR-DEPLOYMENT" prompt "say hello"
```

For full CLI surface, auth options, and session management, see [`USAGE.md`](./USAGE.md).

---

## This fork vs. upstream

| Feature | Upstream (ultraworkers) | This fork |
|---------|------------------------|-----------|
| Azure OpenAI | ❌ | ✅ |
| DashScope (Qwen) | ❌ | ✅ |
| Input-token auto compact | ❌ | ✅ |
| maxInputTokens config | ❌ | ✅ |

---

## Ecosystem

Built in the open alongside the UltraWorkers toolchain:

- [clawhip](https://github.com/Yeachan-Heo/clawhip)
- [oh-my-codex](https://github.com/Yeachan-Heo/oh-my-codex)
- [UltraWorkers Discord](https://discord.gg/5TUQKqFWd)

---

## Disclaimer

- This repository is a fork of [ultraworkers/claw-code](https://github.com/ultraworkers/claw-code).
- It is **not affiliated with, endorsed by, or maintained by** Anthropic or OpenAI.