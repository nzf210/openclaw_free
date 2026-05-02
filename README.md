# AI Software Engineer VPS Stack (LiteLLM + Gemini Edition)

This project provisions an AI agent stack designed to run on a VPS using Docker. 
It is optimized for complex projects (like Scala) using **Gemini 1.5 Pro** via **LiteLLM**.

## Prerequisites
- Docker and Docker Compose installed on your VPS.
- A GitHub Personal Access Token (PAT).
- A **Gemini API Key** (Free from [Google AI Studio](https://aistudio.google.com/)).
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
   # Edit .env with your actual Gemini API Key and Bot Token
   nano .env
   ```

2. **Start the Stack**
   Use Docker Compose to spin up the infrastructure:
   ```bash
   docker-compose up -d
   ```

3. **Verify Connection**
   You can check the logs of LiteLLM to ensure it's connecting to Gemini correctly:
   ```bash
   docker-compose logs -f litellm
   ```

## Why Gemini 1.5 Pro?
For complex Scala projects, Gemini 1.5 Pro offers:
- **Massive Context Window**: Up to 2 million tokens, allowing the agent to read your entire codebase at once.
- **Stability**: Much more reliable than web-scraping alternatives.
- **Free Tier**: High-quality reasoning available without cost for individual developers.

## Troubleshooting
- **Sandbox Performance**: If sbt/JDK is missing in the sandbox, you can ask OpenHands to install it, or we can customize the runtime image in `docker-compose.yml`.
- **Port Conflict**: OpenClaw runs on port `8080`, and LiteLLM on `4000`.
