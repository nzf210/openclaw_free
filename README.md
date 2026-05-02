# AI Software Engineer Stack (OpenClaw + Pi Agent)

This project provisions an AI agent stack designed to run on a VPS or Local Machine using Docker. 
The architecture uses an **embedded Pi Coding Agent** within the **OpenClaw Gateway**, managing its own **sandbox containers** for code execution.

## Multi-Model Support (Device Pairing)
OpenClaw allows you to "pair" your existing web accounts (ChatGPT, Gemini, etc.) via a browser session, enabling you to use premium models for free.

### 1. Pairing a New Model
Execute the following command inside the gateway container to pair a provider:
```bash
docker exec -it openclaw_gateway openclaw device-pair <provider>
```

**Supported Providers:**
- `chatgpt` (OpenAI ChatGPT)
- `gemini` (Google Gemini)
- `kimi` (Moonshot AI)
- `claude` (Anthropic Claude)
- `deepseek` (DeepSeek AI)

### 2. Switching the Active Model
After pairing, update your `.env` file to switch the model used by the Pi Agent:
```env
# Switch between paired models
AC_PI_CODING_AGENT_MODEL=chatgpt:default
# OR
AC_PI_CODING_AGENT_MODEL=gemini:default
# OR
AC_PI_CODING_AGENT_MODEL=kimi:default
```

---

## Setup Instructions

1. **Configure Environment Variables**
   ```bash
   cp .env.example .env
   # Edit .env with your bot token and desired model
   ```

2. **Start the Stack**
   ```bash
   docker compose up -d
   ```

3. **Pairing Messenger (Telegram)**
   ```bash
   docker exec -it openclaw_gateway openclaw pairing approve telegram <OTP_CODE>
   ```

4. **Pairing LLM Models**
   ```bash
   # Pair your ChatGPT account
   docker exec -it openclaw_gateway openclaw device-pair chatgpt
   
   # Pair your Gemini account (Optional)
   docker exec -it openclaw_gateway openclaw device-pair gemini
   ```

## Model Fallback
If your primary model (e.g., ChatGPT) hits a rate limit, the agent will automatically switch to the fallback:
- **Fallback**: `gemini:gemini-1.5-pro` (Configured via `GEMINI_API_KEY`).

## Troubleshooting
- **Pairing Issues**: If the pairing link doesn't work, ensure your VPS has stable internet access.
- **Memory**: 2GB RAM is required for OpenClaw to run browser sessions during pairing.
