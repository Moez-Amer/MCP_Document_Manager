# MCP Document Manager

A Model Context Protocol (MCP) server implementation that provides document management capabilities through tools, resources, and prompts. This project demonstrates how to build an MCP server with FastMCP that can read, edit, and format documents, along with a client for testing the server functionality.

## Features

- **Document Tools**: Read and edit document contents
- **Document Resources**: Access document lists and individual document content
- **Format Prompts**: AI-powered document formatting to Markdown
- **MCP Client**: Test client for interacting with the server
- **MCP Inspector Support**: Debug and inspect server capabilities

## Project Structure

```
├── mcp_server.py      # MCP server implementation with FastMCP
├── mcp_client.py      # MCP client for testing server functionality  
├── main.py           # CLI chat interface (if implemented)
├── core/             # Core application modules
├── pyproject.toml    # Project dependencies and metadata
└── README.md         # This file
```

## Setup

### Step 1: Prerequisites

Ensure you have Python 3.8+ installed on your system.

### Step 2: Install dependencies

#### Option 1: Setup with uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver.

1. Install uv, if not already installed:

```bash
pip install uv
```

2. Create and activate a virtual environment:

```bash
uv venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:

```bash
uv pip install -e .
```

4. Run the project

```bash
uv run python mcp_server.py
```

Or test with the client:

```bash
uv run python mcp_client.py
```

#### Option 2: Setup without uv

1. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

2. Install dependencies:

```bash
pip install -e .
```

3. Run the project

```bash
python mcp_server.py
```

## Usage

### Running the MCP Server

Start the server directly:

```bash
python mcp_server.py
```

The server will start and listen for MCP protocol messages via stdio.

### Testing with MCP Client

Use the included client to test server functionality:

```bash
python mcp_client.py
```

This will connect to the server and demonstrate:
- Listing available tools
- Calling document tools
- Accessing resources

### Using MCP Inspector

For debugging and development, use the official MCP Inspector:

1. Install MCP Inspector:
```bash
npm install -g @modelcontextprotocol/inspector
```

2. Start the inspector with your server:
```bash
mcp-inspector python mcp_server.py
```

3. Open the provided URL in your browser to interact with the server graphically.

### Available Tools

- **read_doc_contents**: Read the contents of a document
- **edit_document**: Edit a document by replacing text

### Available Resources

- **docs://documents**: List all available document IDs
- **docs://documents/{doc_id}**: Get specific document content

### Available Prompts

- **format**: Reformat a document to Markdown syntax

## Document Management

The server includes sample documents:
- `deposition.md` - Legal deposition testimony
- `report.pdf` - Technical condenser tower report
- `financials.docx` - Project budget and expenditures
- `outlook.pdf` - System performance projections
- `plan.md` - Implementation plan
- `spec.txt` - Technical specifications

### Adding New Documents

To add new documents, edit the `docs` dictionary in `mcp_server.py`:

```python
docs = {
    "your_doc.md": "Content of your document here",
    # ... existing documents
}
```

## Development

### Architecture

This project demonstrates the Model Context Protocol (MCP) architecture:

- **MCP Server** (`mcp_server.py`): Implements tools, resources, and prompts using FastMCP
- **MCP Client** (`mcp_client.py`): Connects to the server and demonstrates tool usage
- **Protocol**: Uses stdio transport for communication between client and server

### Extending Functionality

#### Adding New Tools

Add new tools by decorating functions in `mcp_server.py`:

```python
@mcp.tool(
    name="your_tool_name",
    description="Description of what your tool does"
)
def your_tool_function(
    param: str = Field(description="Parameter description")
):
    # Your tool implementation
    return "Result"
```

#### Adding New Resources

Add new resources for data access:

```python
@mcp.resource(
    "your://resource/uri",
    mime_type="application/json"
)
def your_resource_function() -> dict:
    return {"data": "value"}
```

#### Adding New Prompts

Add new prompts for AI interactions:

```python
@mcp.prompt(
    name="your_prompt",
    description="Description of your prompt"
)
def your_prompt_function(
    param: str = Field(description="Parameter description")
) -> list[types.PromptMessage]:
    return [
        types.PromptMessage(
            role="user",
            content=types.TextContent(type="text", text="Your prompt text")
        )
    ]
```

### Testing

Test your server implementation:

1. **Direct testing**: Run `python mcp_client.py` to test tool functionality
2. **Interactive testing**: Use MCP Inspector for visual debugging
3. **Manual testing**: Start the server and send JSON-RPC messages via stdio

### Troubleshooting

#### Common Issues

1. **Import errors**: Ensure all dependencies are installed in your virtual environment
2. **Connection failures**: Check that the server starts without errors
3. **Tool failures**: Verify function signatures match MCP requirements

#### Debugging

- Use MCP Inspector for visual debugging
- Check server logs for error messages
- Ensure proper virtual environment activation

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is for educational and demonstration purposes.

## Learn More

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [FastMCP Documentation](https://github.com/jlowin/fastmcp)
- [MCP Inspector](https://github.com/modelcontextprotocol/inspector)
