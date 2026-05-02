# AI Software Engineer VPS Stack (LiteLLM + ChatGPT OAuth)

This project provisions an AI agent stack designed to run on a VPS using Docker. 
It uses **LiteLLM native ChatGPT OAuth** for GPT access, without a separate reverse-proxy adapter.

## Prerequisites
- Docker and Docker Compose installed on your VPS.
- A GitHub Personal Access Token (PAT).
- A ChatGPT subscription that includes the GPT models you want to use.
- A Bot Token from Telegram, Discord, or Slack.

## Memory Optimization (4GB Cap)
This stack is configured to stay within a **4GB RAM limit** even if the host has more. 
- **Limits**: Containers are restricted via `deploy.resources.limits.memory`.
- **Tuning**: PostgreSQL buffers and Redis max-memory are optimized for a low footprint.
- **Concurrency**: LiteLLM is limited to 2 workers to save memory.

---

## Setup Instructions

1. **Configure Environment Variables**
   Copy the example environment file and fill in your credentials.
   ```bash
   cp .env.example .env
   # Edit .env with your actual keys and bot token
   nano .env
   ```

2. **Start the Stack**
   Use Docker Compose to spin up the infrastructure:
   ```bash
   docker-compose up -d
   ```

3. **Complete LiteLLM ChatGPT Login**
   The first request to a `chatgpt/...` model will trigger LiteLLM's device-code OAuth flow. Watch the LiteLLM logs, open the verification URL, then enter the code shown there.
   ```bash
   docker-compose logs -f litellm
   ```

4. **Pairing Telegram (via Docker Exec)**
   If OpenClaw requires manual pairing for your Telegram account (e.g., OTP login or QR code), you need to execute a command inside the container:
   ```bash
   docker exec -it openclaw_gateway sh
   # Jalankan perintah pairing OpenClaw di sini, misalnya:
   # npm run pair:telegram atau python pair.py
   ```

5. **Verify Connection**
   You can check the logs of LiteLLM to ensure the OAuth login succeeded and requests are reaching the ChatGPT backend:
   ```bash
   docker-compose logs -f litellm
   ```

## Default Models
- `chatgpt/gpt-5.4` is the default model for OpenHands.
- `chatgpt/gpt-5.3-codex` is also registered in LiteLLM if you want a coding-focused alternative.
- OAuth tokens are stored under `./litellm/chatgpt-auth` so they persist across container restarts.

## Troubleshooting
- **Sandbox Performance**: If sbt/JDK is missing in the sandbox, you can ask OpenHands to install it, or we can customize the runtime image in `docker-compose.yml`.
- **First Login**: If GPT requests fail before login, trigger a request from OpenHands and keep `docker-compose logs -f litellm` open until the device code appears.
- **Port Conflict**: OpenClaw runs on port `8080`, OpenHands on `2999`, and LiteLLM on `4000`.
