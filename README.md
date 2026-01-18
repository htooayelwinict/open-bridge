# OpenRouter MCP Bridge

[![PyPI version](https://badge.fury.io/py/openrouter-mcp-bridge.svg)](https://pypi.org/project/openrouter-mcp-bridge/)
![OpenRouter API](https://img.shields.io/badge/OpenRouter-API-blue.svg)

A lightweight MCP (Model Context Protocol) server that enables AI coding assistants to interact with OpenRouter API. Works with Claude Code, Cursor, VS Code, and other MCP-compatible clients.

## ✨ Features

- **Direct OpenRouter API Integration**: Async HTTP calls using httpx
- **Simple MCP Tools**: Three core functions for queries, file analysis, and batch processing
- **Stateless Operation**: No sessions, caching, or complex state management
- **Production Ready**: Robust error handling with configurable timeouts (default: 90 seconds)
- **Minimal Dependencies**: httpx, python-dotenv, and mcp>=1.0.0
- **Async/Await**: Full async support for concurrent operations
- **Model Flexibility**: Support any OpenRouter-compatible model

## 🚀 Quick Start

### Prerequisites

1. **Get OpenRouter API Key**:
   ```bash
   # Visit https://openrouter.ai/keys
   # Sign up and get your API key
   ```

2. **Set Environment Variable**:
   ```bash
   export OPENROUTER_API_KEY="your-api-key-here"
   ```

### Installation

**From PyPI:**
```bash
pip install openrouter-mcp-bridge
```

**Using uvx (recommended for MCP servers):**
```bash
uvx openrouter-mcp-bridge
```

**Local Development:**
```bash
# Clone the repository
git clone https://github.com/htooayelwinict/open-bridge.git
cd open-bridge

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install in development mode
pip install -e .

# Run directly
python -m src
```

## ⚙️ Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `OPENROUTER_API_KEY` | ✅ Yes | - | Your OpenRouter API key |
| `OPENROUTER_MODEL` | No | `openai/gpt-4o` | Model to use |
| `OPENROUTER_TIMEOUT` | No | `90` | Request timeout (seconds) |

### Claude Code Configuration

```bash
# Add to Claude Code with uvx (recommended)
claude mcp add openrouter-mcp-bridge -s user \
  --env OPENROUTER_API_KEY=sk-or-... \
  --env OPENROUTER_MODEL=anthropic/claude-3.5-sonnet \
  -- uvx openrouter-mcp-bridge
```

Or add to `~/.claude.json`:
```json
{
  "mcpServers": {
    "openrouter-mcp-bridge": {
      "type": "stdio",
      "command": "uvx",
      "args": ["openrouter-mcp-bridge"],
      "env": {
        "OPENROUTER_API_KEY": "sk-or-...",
        "OPENROUTER_MODEL": "anthropic/claude-3.5-sonnet"
      }
    }
  }
}
```

## 🛠️ Available Tools

| Tool | Description |
|------|-------------|
| `consult_openrouter` | Direct API query with structured output |
| `consult_openrouter_with_stdin` | Pipe file content for analysis |
| `consult_openrouter_batch` | Multiple concurrent queries |

## 📋 Usage Examples

```python
# Basic query
consult_openrouter(
    query="What authentication patterns are used in this project?",
    directory="/path/to/project",
    format="json"
)

# File analysis
consult_openrouter_with_stdin(
    stdin_content=open("src/auth.py").read(),
    prompt="Review this file for security issues",
    directory="/path/to/project"
)

# Batch processing
consult_openrouter_batch(
    queries=[
        {"query": "Analyze authentication patterns"},
        {"query": "Review database implementations"}
    ],
    directory="/path/to/project"
)
```

## 🏗️ Architecture

- **API-First**: Direct async HTTP calls to OpenRouter API
- **Stateless**: Each tool call is independent with no session state
- **Async/Await**: Full async support for concurrent operations
- **Error Handling**: Comprehensive HTTP and timeout error handling

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Credits

Originally forked from [shelakh/codex-bridge](https://github.com/shelakh/codex-bridge).

---

**Focus**: A simple, reliable bridge between Claude Code and OpenRouter API.
