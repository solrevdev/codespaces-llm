# codespaces-llm

GitHub Codespaces environment with LLM CLI tool, Python 3.13, `uv` package manager, and GitHub Copilot VS Code extension for accessing GitHub Models API.

**ALWAYS reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Working Effectively

### Bootstrap and Setup
Run these commands in sequence to set up the environment:

```bash
pip install uv
```
- Takes ~4 seconds. NEVER CANCEL.

```bash
uv tool install llm
```
- Takes ~46 seconds. NEVER CANCEL. Set timeout to 90+ seconds.

```bash
llm install llm-github-models
```
- Takes ~19 seconds. NEVER CANCEL. Set timeout to 60+ seconds.

```bash
llm models default github/gpt-4o
```
- Takes ~1 second. Sets the default model to GPT-4o via GitHub Models.

### Authentication Setup
**CRITICAL**: LLM requires GitHub authentication to access GitHub Models:

- In GitHub Codespaces: GitHub token may be automatically available via GITHUB_TOKEN environment variable
- In local environments: Set GitHub token using one of these methods:
  - `llm keys set github` (interactive prompt - will ask for key input)
  - Set `GITHUB_TOKEN` environment variable
  - Set `GITHUB_MODELS_KEY` environment variable

**Note**: Without proper authentication, LLM queries will fail with "No key found" error.

### Running LLM Queries
After setup, test functionality with:

```bash
llm "Fun facts about pelicans"
```

This is the primary use case for this repository.

## Validation

### Manual Testing Requirements
ALWAYS test the complete workflow after making changes:

1. **Setup Validation**: Run all bootstrap commands and verify no errors
2. **Authentication Check**: Ensure GitHub token is configured
3. **Functionality Test**: Execute `llm "Fun facts about pelicans"` successfully
4. **Model Verification**: Run `llm models list` and confirm GitHub models are available

### Expected Behavior
- Default model should be set to `github/gpt-4o`
- LLM should return actual AI-generated responses about pelicans (when authenticated)
- Commands should complete without authentication errors (when properly set up)

### Limitations
- **Authentication Required**: Cannot test actual LLM functionality without valid GitHub token
- **Network Dependent**: All installations require internet connectivity
- **Environment Specific**: Designed for GitHub Codespaces environment

## Repository Structure

### Key Files
```
/
├── README.md                     # Main documentation
├── .devcontainer/
│   └── devcontainer.json        # Codespaces configuration
└── .github/
    └── copilot-instructions.md  # This file
```

### Environment Details
- **Python**: 3.13 (via devcontainer base image, but may be 3.12.x in some environments)
- **Node.js**: v20+ (installed via devcontainer feature)
- **Package Manager**: `uv` for Python tools
- **Main Tool**: `llm` CLI for accessing language models
- **Plugin**: `llm-github-models` for GitHub Models API access

**Important**: Plugin installations are separate from tool installations and do not persist across tool reinstalls.

## Common Tasks

### Check Installation Status
```bash
which uv          # Verify uv is installed
which llm         # Verify llm is installed
llm plugins       # List installed plugins
llm models list   # Show available models
llm keys list     # Show configured API keys
```

### Available Models
The repository provides access to GitHub Models including:
- GitHub Models: github/gpt-4o (default)
- GitHub Models: github/gpt-4o-mini
- GitHub Models: github/o1-mini
- GitHub Models: github/Meta-Llama-3.1-405B-Instruct
- And many others via GitHub Models API

### Troubleshooting Commands
```bash
llm --version               # Check LLM version
python3 --version          # Verify Python version
uv tool list               # Show installed tools (LLM installed via uv tool)
llm plugins                # Verify llm-github-models plugin
llm models | grep "Default:"  # Check default model setting
```

## Development Notes

### No Build Process
This repository has NO build process, compilation steps, or CI workflows. It's a pure environment setup.

### No Tests
This repository contains no unit tests or test suites to run.

### Dependencies
All dependencies are Python packages installed via `pip` and `uv`:
- `uv`: Modern Python package installer
- `llm`: CLI tool for accessing language models
- `llm-github-models`: Plugin for GitHub Models integration

### Authentication Flow
1. In Codespaces: GitHub token automatically available
2. Local development: Manual token configuration required
3. First run: May require `llm keys set github` command

## Time Expectations

- **Complete setup**: ~70 seconds total
- **pip install uv**: ~4 seconds
- **uv tool install llm**: ~46 seconds  
- **llm install plugin**: ~19 seconds
- **Set default model**: ~1 second
- **LLM query execution**: 5-15 seconds (depending on response length)

**NEVER CANCEL** any of these operations. Always set appropriate timeouts (90+ seconds for tool installations).

## Error Scenarios

### Common Errors and Solutions

**"No key found" error**:
```bash
# Solution: Set GitHub token
llm keys set github
# Or verify GITHUB_TOKEN environment variable
```

**"command not found: llm"**:
```bash
# Solution: Ensure uv tool installed llm correctly
uv tool install llm
# Verify PATH includes ~/.local/bin
echo $PATH | grep ".local/bin"
```

**Plugin not found**:
```bash
# Solution: Reinstall plugin
llm install llm-github-models
```

**Plugin Persistence Issue**:
```bash
# If you reinstall llm tool, plugins are lost and must be reinstalled
uv tool install --reinstall llm  # This will remove all plugins
llm install llm-github-models    # Must reinstall plugin after
```

**Network timeouts during installation**:
- Package installations may timeout due to network issues
- Retry the command if installation fails with timeout errors
- NEVER CANCEL during normal installation progress
- If network issues persist, the environment may not be fully functional

**Environment Issues**:
```bash
# Check if tools are properly installed
uv tool list        # Should show: llm v0.27.1
llm plugins         # Should show llm-github-models
which uv           # Should show: /home/runner/.local/bin/uv
which llm          # Should show: /home/runner/.local/bin/llm
```

Always run the complete bootstrap sequence if encountering setup issues.