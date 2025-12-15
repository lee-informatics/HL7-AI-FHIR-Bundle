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
  - Install Ollama from [ollama.ai](https://ollama.ai)
  - Pull a model (e.g., `ollama pull llama3.2` or `ollama pull qwen3:4b` or other)

## Required Configuration Files

The following configuration files must be present in the project directory:

1. **`settings.yml`** - SearXNG search engine configuration
2. **`mcpo_config.json`** - MCP server orchestrator configuration for connecting to Crawl4AI and other MCP servers

## Starting the Services

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

- **Open WebUI**: http://localhost:3080 - Main AI chat interface
- **HAPI FHIR Server**: http://localhost:8081 - FHIR RESTful API
- **SearXNG**: http://localhost:8080 - Search engine interface
- **Crawl4AI**: http://localhost:11235 - Web crawling service
- **MCPO**: http://localhost:8000 - MCP Orchestrator
- **PostgreSQL**: localhost:5432 - Database (credentials: postgres/password)

## Configuring Open WebUI

After starting the services, you'll need to configure Open WebUI to enable web search and MCP tools:

### Enable Web Search in Open WebUI
- Click ***User*** in the bottom left corner
- Click Admin Panel
- Click the Settings header on the top
- Click Web Search from the menu
- Add ```http://host.docker.internal:8080/search?q=<query>``` to the *SearXNG Query URL* field
- Click Save

### Enable MCP Servers
- In the Admin Panel Settings menu, click External Tools
- Click the + icon
- Click the import button and choose one of the [MCP Tools](mcp_tools/). Click Save and repeat for every tool.
- Alternative: Manual Configuration
  - For HAPI FHIR MCP
    - Select **MCP Streamable HTTP** as the type
    - Add ```http://host.docker.internal:8081/mcp/messages``` to the URL field
    - ID: hapi-fhir
    - Name: HAPI FHIR MCP
    - Leave the rest blank or default and save
  - For the Crawl4AI MCP
    - Go back and click the + icon again
    - Select **OpenAPI** as the type
    - Add ```http://host.docker.internal:8000/crawl4ai``` to the URL field
    - ID: c4ai
    - Name: crawl4ai
    - Leave the rest blank or default and save