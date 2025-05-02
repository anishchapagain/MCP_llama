# MCP_llama - Model Context Protocol using LLama

# MCP File System Server

A server that provides a Model Context Protocol (MCP) interface to the local file system, allowing Ollama to access files as context for generating responses.

## Features

- 📂 **File System Access**: Provide local files as context to language models
- 🧠 **MCP Integration**: Structured context following Model Context Protocol
- 🔄 **Ollama Integration**: Seamless connection to Ollama's API
- 💬 **Interactive Mode**: Chat with context from files in a session
- 🌐 **RESTful API**: Simple HTTP endpoints for programmatic access

## Installation

### Prerequisites

- Python 3.8+
- Ollama installed and running

### Setup

1. Clone this repository or copy the files
2. Install dependencies:

```bash
pip install fastapi uvicorn requests pydantic rich
or
uv ....... ?
```

## Usage

### Starting the Server

```bash
python mcp_server.py --host 127.0.0.1 --port 8000
```

Options:
- `--host`: Host address (default: 127.0.0.1)
- `--port`: Port number (default: 8000)
- `--ollama-url`: Ollama API endpoint (default: http://localhost:11434/api/chat)
- `--file-limit`: Maximum file size in MB (default: 5)
- `--context-limit`: Maximum total context size in MB (default: 10)

### Using the Client

The client provides three main modes of operation:

#### 1. List Files

List files matching a pattern:

```bash
python mcp_client.py list /path/to/directory --pattern "*.py"
```

#### 2. One-time Query

Send a query with file context:

```bash
python mcp_client.py query \
  --model llama3 \
  --query "What does this code do?" \
  --file /path/to/file1.py \
  --file /path/to/file2.py \
  --system "You are a helpful coding assistant"
```

Use patterns to include multiple files:

```bash
python mcp_client.py query \
  --model llama3 \
  --query "Summarize these Python files" \
  --pattern "/path/to/src/*.py"
```

#### 3. Interactive Session

Start an interactive chat session with persistent context:

```bash
python mcp_client.py interactive --model llama3
```

In interactive mode, use the following commands:
- `/files add <path>` - Add file(s) to context
- `/files clear` - Clear file context
- `/files list` - List current context files
- `/system <prompt>` - Set system prompt
- `/clear` - Clear conversation history
- `/exit` - Exit the session

## API Endpoints

### File Operations

- `POST /files/list` - List files matching a pattern
- `POST /files/read` - Read file contents

### MCP Operations

- `POST /mcp/query` - Query LLM with file context using MCP
- `POST /mcp/raw` - Send raw MCP request to Ollama

## Example Workflows

### Code Understanding

```bash
# Add a project directory to context
python mcp_client.py interactive
> /files add ./my-project/src
> Explain what this codebase does and its architecture
```

### Document Analysis

```bash
# Analyze multiple documents
python mcp_client.py query \
  --query "Compare and contrast these articles" \
  --file ./documents/article1.txt \
  --file ./documents/article2.txt
```

### Technical Support

```bash
# Debug errors with context from log files
python mcp_client.py interactive
> /files add ./logs/error.log
> /files add ./config/settings.json
> What might be causing the errors in these logs and how can I fix them?
```

## Advanced Usage

### Custom System Prompts

Use system prompts to guide the model's behavior:

```bash
python mcp_client.py query \
  --query "Explain this code" \
  --file ./src/main.py \
  --system "You are an expert Python developer specializing in performance optimization. Analyze the code with a focus on efficiency and potential bottlenecks."
```

### File Patterns

Use glob patterns to include multiple files:

```bash
python mcp_client.py query \
  --query "What's the overall architecture?" \
  --pattern "./src/**/*.js" \
  --pattern "./docs/*.md"
```

## Troubleshooting

- **File Too Large**: Increase the file size limit with `--file-limit`
- **Context Too Large**: Increase the context limit with `--context-limit` or reduce the number of files
- **Connection Error**: Ensure Ollama is running and accessible at the specified URL

## Limitations

- Binary files are not supported
- Very large files may be truncated
- Total context size is limited by the model's maximum context window
