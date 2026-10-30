# The Toolbox MCP

🚀 **AI Agent Tool Discovery via Librarian + Tavily**

An MCP (Model Context Protocol) server that enables AI agents to discover installed tools and packages using Librarian's system-wide index, enriched with real-time web intelligence from Tavily.

---

## ✨ Features

- **Tool Discovery:** Query 9,236+ tools across 14 package managers
- **Full-Text Search:** Millisecond-fast FTS5 queries
- **Tavily Integration:** Real-time tool research and descriptions
- **Safe:** Read-only, bounded, no tool execution
- **MCP Native:** Works with Claude, Bifang, and other MCP clients

---

## 🛠️ Tools Provided

### 1. `toolbox_search`
Search for tools by keywords with pagination.

### 2. `toolbox_describe`  
Get detailed info for a specific tool.

### 3. `toolbox_inventory`
Overview of indexed tools, sources, and categories.

### 4. `toolbox_research` 🆕 (Tavily)
Enrich tool information with real-time web search via Tavily.

---

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- `uv` package manager
- Librarian index database
- Tavily API key (optional, for enrichment)

### Setup

```bash
# Clone this repo
git clone https://github.com/Protocol-X-01/toolbox-mcp.git
cd toolbox-mcp

# Install dependencies
uv sync --locked

# Set environment variables
export TAVILY_API_KEY='tvly_your_key_here'

# Run the server
uv run --frozen the-toolbox --db /path/to/librarian/index.db
```

---

## 🔍 Tavily Integration

The Toolbox MCP uses **Tavily** instead of Bright Data for tool enrichment:

### Benefits

✅ **Real-time Research:** Current documentation and examples  
✅ **High Quality:** LLM-optimized results  
✅ **Affordable:** Pay-per-use, no subscription  
✅ **Simple:** One API key, works everywhere  
✅ **Transparent:** Clear sources and citations  

### How It Works

When `toolbox_research` is called:

1. Search Librarian index for tool
2. Query Tavily for latest info
3. Combine indexed + web data
4. Return enriched result

### Example

```python
# Search for a tool
result = toolbox_search("sql injection")

# Enrich with Tavily
enriched = toolbox_research("sqlmap")
# Returns:
{
  "name": "sqlmap",
  "source": "pacman",
  "category": "webapp",
  "version": "1.10.5-1",
  "path": "/usr/bin/sqlmap",
  "tavily_enriched": {
    "description": "Automatic SQL injection and database takeover tool",
    "github": "https://github.com/sqlmapproject/sqlmap",
    "documentation": "https://sqlmap.org",
    "last_updated": "2026-10-29"
  }
}
```

---

## 📊 Stats

**Current Index (as of 2026-10-29):**
- **Total Tools:** 9,236
- **Sources:** 14 package managers
- **Categories:** 60+ types
- **Query Time:** <8ms average

---

## 🔗 Integration

### With Bifang

```bash
export BIFANG_MCP_SERVERS=[{
  "name":"the-toolbox",
  "command":"/path/to/toolbox-mcp/.venv/bin/the-toolbox",
  "args":["--db","/var/lib/librarian/index.db"]
}]
```

### With Claude Desktop

```json
{
  "mcpServers": {
    "the-toolbox": {
      "command": "/path/to/toolbox-mcp/.venv/bin/the-toolbox",
      "args": ["--db", "/var/lib/librarian/index.db"],
      "env": {
        "TAVILY_API_KEY": "tvly_your_key_here"
      }
    }
  }
}
```

---

## 🎯 Why Tavily?

**vs Bright Data:**

| Feature | Tavily | Bright Data |
|---------|--------|-------------|
| Setup | 1 API key | Complex proxy |
| Quality | LLM-optimized | Raw SERP |
| Cost | Pay-per-use | Subscription |
| Rate Limits | Generous | Strict |
| Maintenance | Easy | Complex |
| Agent-Friendly | ✅ Yes | ⚠️ Manual |

**Winner:** Tavily for AI agents! 🏆

---

## 🔧 Configuration

### Environment Variables

```bash
# Required for Tavily enrichment
TAVILY_API_KEY=tvly_xxxxx

# Optional: Custom LangSmith tracing
LANGSMITH_API_KEY=lsv2_pt_xxxxx
LANGSMITH_PROJECT=toolbox-mcp
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
```

### Index Path

The Toolbox reads from Librarian's SQLite index:

```bash
# Default path
/var/lib/librarian/index.db

# Or specify custom path
uv run the-toolbox --db /custom/path/index.db
```

---

## 📝 API Reference

### toolbox_search

```python
toolbox_search(
    query: str,           # Search terms (1-256 chars)
    source: str = None,   # Filter by package manager
    category: str = None, # Filter by category
    limit: int = 15,      # Results per page (1-50)
    offset: int = 0       # Pagination (0-10000)
) -> dict
```

### toolbox_describe

```python
toolbox_describe(
    name: str  # Exact tool name (1-256 chars)
) -> dict
```

### toolbox_inventory

```python
toolbox_inventory() -> dict
```

### toolbox_research 🆕

```python
toolbox_research(
    name: str  # Tool name to enrich via Tavily
) -> dict
```

---

## 🛡️ Safety Features

✅ **Read-Only:** SQLite opened with `mode=ro`  
✅ **Bounded Inputs:** Max query length, pagination limits  
✅ **Timeout Protection:** 1-second query deadline  
✅ **No Execution:** Discovers tools, doesn't run them  
✅ **Untrusted Data:** Descriptions are evidence, not instructions  

---

## 🧪 Testing

```bash
# Run tests
uv run pytest tests/ -v

# Test Tavily integration
export TAVILY_API_KEY='your_key'
uv run python -c "
from toolbox import toolbox_research
result = toolbox_research('nmap')
print(result)
"
```

---

## 📦 Supported Sources

Librarian indexes tools from:

- **pacman** (Arch + BlackArch)
- **aur** (AUR packages)
- **path** ($PATH binaries)
- **cargo** (Rust)
- **pip/pipx** (Python)
- **npm/yarn/pnpm** (JavaScript)
- **docker** (Container images)
- **desktop** (GUI apps)
- **foundry** (Solidity)
- **conda** (Data science)
- **rpm** (RPM packages)
- And more...

---

## 🤝 Integration with Bifang

Built for the **Nebius x NVIDIA Hackathon 2026**:

- **Repo:** https://github.com/Protocol-X-01/Bifang
- **Live Test:** Nemotron Nano roundtrip verified
- **Tokens:** 8,885 tracked in live demo
- **Status:** Production ready ✅

---

## 🎓 Use Cases

### 1. AI Coding Agents
"Find tools for SQL injection testing"
→ Returns: sqlmap, jsql-injection, sqlbrute...

### 2. Security Researchers  
"What wireless tools do I have?"
→ Returns: aircrack-ng, wifite, reaver...

### 3. DevOps Engineers
"Find container tools"
→ Returns: docker, podman, buildah...

### 4. Data Scientists
"What ML libraries are installed?"
→ Returns: numpy, pandas, scikit-learn...

---

## 🔗 Links

- **Repository:** https://github.com/Protocol-X-01/toolbox-mcp
- **Librarian:** https://github.com/Protocol-X-01/librarian
- **Tavily:** https://tavily.com
- **MCP Protocol:** https://modelcontextprotocol.io

---

## 📄 License

MIT License - See [LICENSE](LICENSE)

---

## 🙏 Credits

- **Librarian** - System-wide tool indexing
- **Tavily** - Web intelligence API
- **MCP** - Model Context Protocol
- **Bifang** - AI coding agent integration

Built with ❤️ for the Nebius x NVIDIA Hackathon 2026
