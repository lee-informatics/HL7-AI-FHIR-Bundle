# AI-FHIR-Bundle

A complete local AI setup with FHIR (Fast Healthcare Interoperability Resources) integration, web search capabilities, and MCP (Model Context Protocol) server support. This project enables you to run a fully functional local AI environment with healthcare data interoperability, leveraging LLM agents with access to FHIR resources and web search tools.

## What's Included

This docker-compose setup provides:

- **HAPI FHIR Server (R4)** - A complete FHIR server with MCP integration for healthcare data operations
- **PostgreSQL** - Database backend for HAPI FHIR
- **Open WebUI** - Modern chat interface for interacting with local LLMs
- **Ollama Integration** - Connects to your locally running Ollama instance for LLM inference
- **SearXNG** - Privacy-focused metasearch engine for web search capabilities
- **Crawl4AI** - Advanced web crawling and extraction service with LLM processing
- **MCPO** - MCP Orchestrator/Proxy for managing MCP server connections

## Prerequisites

- Docker and Docker Compose installed
- **Ollama running locally on port 11434** - You must have Ollama installed and running before starting this stack
  - **Installation options:**
    - Download and install from [ollama.ai](https://docs.ollama.com/quickstart), OR
    - If you are on macOS with Homebrew installed: ```brew install ollama```
  - **Start Ollama:**
    - If installed via ollama.ai: Follow their instructions to start the service
    - If installed via Homebrew on macOS: The service starts automatically, or run ```ollama serve```
  - **Pull a model:**
    - Run ```ollama pull qwen3:14b``` or any other model of your choice

## Required Configuration Files

The following configuration files must be present in the project directory:

1. [searXNG_settings.yml](searXNG_settings.yml) - SearXNG search engine configuration
2. [mcpo_config.json](mcpo_config.json) - MCP server orchestrator configuration for connecting to Crawl4AI and other MCP servers

## Starting the Services

### Initial Setup

1. **Configure environment variables:**
  - Copy `.env.example` to `.env`
  - If Ollama is running on a remote server, update the `OLLAMA_BASE_URL` in `.env` with your server URL

### Start Services

To start all services with the latest images and clean up any orphaned containers:

```bash
docker compose up --pull always --remove-orphans
```

This command will:
- Pull the latest versions of all container images
- Remove any orphaned containers from previous runs
- Start all services defined in the docker-compose.yml

## Access Points

Once running, you can access:

- **Open WebUI**: http://localhost:3000 - Main AI chat interface
- **HAPI FHIR Server**: http://localhost:8081 - FHIR RESTful API
- **SearXNG**: http://localhost:8080 - Search engine interface
- **Crawl4AI**: http://localhost:11235 - Web crawling service
- **MCPO**: http://localhost:8000 - MCP Orchestrator
- **PostgreSQL**: localhost:5432 - Database (credentials: postgres/password)

## Using Open WebUI

Vist http://localhost:3000 and you are ready to go. Just remember to toggle on websearch and any mcp tools needed for each chat.

### Recommended
- In Open WebUI:
  1. Click "User" (bottom-left) → "Admin Panel" → "Settings" → "Models".
  2. Select the model you're using and add [model_prompt.txt](model_prompt.txt).

- If tools are not working great, add the following under "External tools":

  - HAPI-FHIR  
    - Function Name Filter List (comma-separated):
    ```text
    tool_create_fhir_resource_post, tool_read_fhir_resource_post, tool_update_fhir_resource_post, tool_delete_fhir_resource_post, tool_search_fhir_resources_post
    ```

  - Crawl4AI  
    - Function Name Filter List:
    ```text
    tool_crawl_post
    ```

