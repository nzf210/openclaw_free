# AI Software Engineer VPS Stack (GPT4Free Edition)

This project provisions an AI agent stack designed to run on a VPS using Docker. 
It integrates the following services:
- **OpenHands**: The core AI Software Engineer agent.
- **OpenClaw**: A gateway to interface with messaging platforms (Telegram, Discord, Slack).
- **GPT4Free (g4f)**: A reverse-proxy container that provides an OpenAI-compatible API without needing an API key (by scraping free web providers).
- **PostgreSQL (pgvector)**: Persistent storage and vector database for RAG/memory.
- **Redis**: Caching and background queueing.

## Prerequisites
- Docker and Docker Compose installed on your VPS.
- A GitHub Personal Access Token (PAT).
- A Bot Token from Telegram (via BotFather), Discord, or Slack.

## Setup Instructions

1. **Configure Environment Variables**
   Copy the example environment file and fill in your credentials.
   ```bash
   cp .env.example .env
   # Edit .env with your favorite editor (e.g., nano .env)
   ```

2. **Start the Stack**
   Use Docker Compose to spin up the infrastructure:
   ```bash
   docker-compose up -d
   ```
   This will start PostgreSQL, Redis, g4f, OpenHands, and OpenClaw. 

3. **Accessing the g4f Web UI**
   You can access the GPT4Free Web UI at `http://<your-vps-ip>:8080/chat/` to test models manually. The internal API runs on port 1337 and is already configured for OpenHands.

## GitHub Workflow & Source of Truth

**OpenHands** acts as an autonomous developer. To maintain a clean workflow:

1. Provide your `GITHUB_TOKEN` in the `.env` file.
2. The `workspace` directory (mounted at `./workspace`) is available to OpenHands.
3. When giving a task via Telegram/Discord, instruct OpenHands to:
   - Clone your target repository into the workspace.
   - Create a new branch for the feature/fix.
   - Write and test the code within its Sandbox.
   - Commit the changes and push the branch to GitHub.
   - (Optional) Open a Pull Request using the GitHub CLI or API.

GitHub remains the absolute source of truth for your source code, while the VPS and Docker Sandbox serve strictly as the agent's execution and testing environment.

## Troubleshooting & Warnings

> **WARNING**: GPT4Free relies on web scraping. If free providers update their systems, block your IP, or experience high traffic, the API will fail or become very slow. 

- **Checking g4f Logs**: 
  If OpenHands stops responding or errors out during reasoning, check if `g4f` is failing to fetch responses:
  ```bash
  docker-compose logs -f g4f
  ```
- **OpenHands Sandbox Access**:
  OpenHands relies on access to the Docker socket to spawn sub-containers for executing generated code safely. Make sure `/var/run/docker.sock` permissions allow the container to use it.
