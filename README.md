
# AI-FHIR-Bundle

A local, docker-compose based stack that integrates a FHIR server, LLM tooling, web search, and MCP (Model Context Protocol) orchestration—designed for building and testing AI agents that interact with FHIR resources and external web content.

## Features

- HAPI FHIR Server (R4) with MCP integration
- PostgreSQL database for persistence
- Open WebUI — chat interface for interacting with local LLMs
- Ollama integration for local LLM inference
- SearXNG — privacy-focused metasearch engine
- Crawl4AI — web crawling and content ingestion service
- fhir-mcp-server - access to HL7 Terminolgy server
- MCPO — MCP Orchestrator/Proxy to manage MCP connections

## Prerequisites

- Docker and Docker Compose
- Ollama (if you want local LLM inference)
  - Install: see https://docs.ollama.com/quickstart or, on macOS with Homebrew, `brew install ollama`
  - Start: follow Ollama docs or run `ollama serve` if needed
  - Pull a model (example):

To have the best experience you need a GPU with enough RAM to completely load your model of choice. Our tests and default configuration use the qwen3-coder-next model that uses 52GB of GPU RAM!
```bash
ollama pull qwen3-coder-next:latest
```

## Configuration files

Place the following files in the project root (examples may be included in the repo):

- `searXNG_settings.yml` — SearXNG configuration
- `mcpo_config.json` — MCP orchestrator configuration for connecting to Crawl4AI and other MCP servers

## Quick start

1. Copy the environment file and edit values if needed:

```bash
cp .env.example .env
# Edit .env if Ollama or other services run on non-default hosts/ports
```

2. Start the full stack (pulls latest images and removes orphaned containers):

```bash
docker compose up --pull always --remove-orphans
```

This will pull the required images, remove orphaned containers, and start all services defined in `docker-compose.yml`.

## Service endpoints

- Open WebUI: http://localhost:3000
- SearXNG: http://localhost:8080
- HAPI FHIR Server (R4): http://localhost:8081
- Crawl4AI: http://localhost:11235
- fhir-mcp-server: http://localhost:8001
- MCPO: http://localhost:8000
- PostgreSQL: localhost:5432 (default: `postgres` / `password`)

## Using Open WebUI

1. Open http://localhost:3000
2. In the UI: User → Admin Panel → Settings → Models
   - Select the model you pulled in Ollama and add `model_prompt.txt` as the model prompt
3. Under Tools, enable the integrations: `crawl4ai`, `HAPI FHIR`, `terminology-server`
4. Save and update settings

Notes:
- Only enable Web Search when you need live web queries — it triggers searches for every requests and can slow interactions.
- Use Crawl4AI when you need deep search as scraping and ingestion takes a long time.

## Tips & Troubleshooting

- Prevent the LLM from accidentally trying to change the terminology server as you probably not intending to and the provided server is read only anyways: in Open WebUI go to User → Admin Panel → Settings → External tools → terminology-server and add the following to the Function Name Filter List (comma-separated):

```text
!tool_create_post, !tool_update_post, !tool_delete_post
```

- If crawls fail frequently, add `tool_crawl_post` to the Function Name Filter List.

## License

See the `LICENSE` file for license details.

