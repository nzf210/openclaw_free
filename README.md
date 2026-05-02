# AI Software Engineer Stack (OpenClaw + Pi Agent)

This project provisions an AI agent stack designed to run on a VPS or Local Machine using Docker. 
The new architecture uses an **embedded Pi Coding Agent** within the **OpenClaw Gateway**, managing its own **sandbox containers** for code execution.

## Architecture
- **OpenClaw Gateway**: Main container for messaging (Telegram, Discord, Slack).
- **Pi Coding Agent**: Embedded agent plugin for automated software engineering.
- **Sandbox Containers**: Dynamic Docker containers for secure code execution.
- **Workspace**: Local `workspace/` directory mounted into the agent system.

## Prerequisites
- Docker and Docker Compose installed.
- A Telegram Bot Token (or Discord/Slack).
- OpenAI / Gemini API Key (for fallback).
- A ChatGPT account (for primary OAuth pairing).

## Memory Optimization (4GB Cap)
This stack is configured to stay within a **4GB RAM limit**:
- **OpenClaw**: Allocated **2GB** to handle plugin staging and browser sessions.
- **PostgreSQL**: Optimized with low shared buffers for a small footprint.
- **Redis**: Limited to 200MB max memory.

---

## Setup Instructions

1. **Configure Environment Variables**
   ```bash
   cp .env.example .env
   # Edit .env with your actual keys and bot token
   ```

2. **Start the Stack**
   ```bash
   docker compose up -d
   ```

3. **Pairing Telegram**
   ```bash
   docker exec -it openclaw_gateway openclaw pairing approve telegram <OTP_CODE>
   ```

4. **Pairing ChatGPT (OAuth/Browser Login)**
   ```bash
   docker exec -it openclaw_gateway openclaw device-pair chatgpt
   ```
   *Follow the provided link to complete the login.*

## Model Fallback
- **Primary**: `chatgpt:default` (Paired ChatGPT account).
- **Fallback**: `gemini:gemini-1.5-pro` (Configured via `GEMINI_API_KEY`).

## Troubleshooting
- **Memory Errors**: If OpenClaw hits OOM during plugin install, ensure the host has enough swap or increase the memory limit in `docker-compose.yml`.
- **Sandbox Access**: OpenClaw requires access to `/var/run/docker.sock` to spawn sandboxes.
