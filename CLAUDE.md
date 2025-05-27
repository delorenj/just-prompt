# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Development
- **Install dependencies**: `mise run install` or `uv sync`
- **Run tests**: `mise run test`
- **Run the server locally**: `mise run dev`
- **List available models**: `mise run list-models`
- **Clean up generated files**: `mise run clean`

### Testing
- **Run all tests**: `mise run test`
- **Run specific test file**: `uv run pytest src/just_prompt/tests/molecules/test_prompt.py`
- **Run tests with verbose output**: `mise run test-verbose`
- **Show project structure**: `mise run tree`

## Architecture

This is an MCP (Model Control Protocol) server that provides a unified interface to various LLM providers. The codebase follows an atoms/molecules architecture:

### Core Components

**Atoms** (`src/just_prompt/atoms/`):
- **LLM Providers**: Individual provider implementations (OpenAI, Anthropic, Gemini, Groq, DeepSeek, Ollama)
- **Shared Components**:
  - `model_router.py`: Routes requests to appropriate providers based on model prefix
  - `validator.py`: Validates API keys and provider availability
  - `data_types.py`: Pydantic models and enums used throughout the codebase
  - `utils.py`: Helper functions for model parsing and provider shortcuts

**Molecules** (`src/just_prompt/molecules/`):
- Higher-level functions that implement the MCP tools:
  - `prompt.py`: Send prompts to multiple models
  - `prompt_from_file.py`: Read prompts from files
  - `prompt_from_file_to_file.py`: Save responses to files
  - `ceo_and_board_prompt.py`: Multi-model decision-making tool
  - `list_providers.py` / `list_models.py`: Provider/model discovery

**Server** (`src/just_prompt/server.py`):
- MCP server implementation that exposes molecules as tools
- Handles stdio communication with Claude Code

### Key Patterns

1. **Provider Prefixes**: All models must be prefixed with provider shorthand (e.g., `o:gpt-4o`, `a:claude-3-5-haiku`)
2. **Model Router**: Automatically validates and corrects model names using the first default model
3. **Special Model Suffixes**:
   - OpenAI reasoning models: `:low`, `:medium`, `:high` (e.g., `o:o3:high`)
   - Anthropic thinking tokens: `:1k`, `:4k`, `:8k` (e.g., `a:claude-3-7-sonnet-20250219:4k`)
   - Gemini thinking budget: `:1k`, `:4k`, `:8k` (e.g., `g:gemini-2.5-flash-preview-04-17:4k`)

### Environment Variables

The server requires API keys to be set as environment variables:
- `OPENAI_API_KEY`
- `ANTHROPIC_API_KEY`
- `GEMINI_API_KEY`
- `GROQ_API_KEY`
- `DEEPSEEK_API_KEY`
- `OLLAMA_HOST` (for local Ollama server)

The server will start even if some keys are missing, but only providers with valid keys will be available.