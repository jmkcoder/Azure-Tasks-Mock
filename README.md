
# Azure Pipeline Emulator

> ⚡ **Alpha Release** - Run and debug Azure Pipelines locally before pushing to Azure DevOps.

A powerful local development tool that emulates Azure DevOps Pipelines on your machine. Test your CI/CD workflows instantly without waiting for cloud builds.

## 🚀 Quick Start

**Linux/macOS:**
```bash
docker run -d \
  --name azure-pipeline-emulator \
  -p 4200:80 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /path/to/workspace:/workspace \
  -e HOST_WORKSPACE_PATH=/path/to/workspace \
  jmkcoder/azuretasksmock-azure-pipeline-emulator:latest
```

**Windows (PowerShell):**
```powershell
$env:HOST_WORKSPACE_PATH = "C:\path\to\workspace"

docker run -d `
  --name azure-pipeline-emulator `
  -p 4200:80 `
  -v //var/run/docker.sock:/var/run/docker.sock `
  -v "$($env:HOST_WORKSPACE_PATH):/workspace" `
  -e "HOST_WORKSPACE_PATH=$env:HOST_WORKSPACE_PATH" `
  jmkcoder/azuretasksmock-azure-pipeline-emulator:latest
```

Open **http://localhost:4200** to access the web interface.

## ✨ Key Features

- **Local Pipeline Execution** - Run `azure-pipelines.yml` files without pushing to Azure DevOps
- **Multi-Stage Pipelines** - Full support for stages, jobs, and step dependencies
- **Real-time Logs** - Stream pipeline output as it executes
- **Debugging** - Set breakpoints before or after any step
- **Artifact Support** - Publish and download artifacts between stages
- **Task Emulation** - Built-in support for common Azure DevOps tasks

## 🐳 Docker Compose Setup

```yaml
services:
  azure-pipeline-emulator:
    image: jmkcoder/azuretasksmock-azure-pipeline-emulator:latest
    container_name: azure-pipeline-emulator
    ports:
      - "4200:80"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ${HOST_WORKSPACE_PATH}:/workspace
    environment:
      - Pipeline__HostWorkspaceDirectory=${HOST_WORKSPACE_PATH}
    restart: unless-stopped
```

**Linux/macOS:**
```bash
export HOST_WORKSPACE_PATH=/path/to/workspace && docker-compose up -d
```

**Windows (PowerShell):**
```powershell
$env:HOST_WORKSPACE_PATH = "C:\path\to\workspace"; docker-compose up -d
```

## ⚙️ Configuration

| Variable | Description |
|----------|-------------|
| `HOST_WORKSPACE_PATH` | Host machine path for pipeline workspaces (required on Windows) |

| Volume | Description |
|--------|-------------|
| `/var/run/docker.sock` | Docker socket access (required) |
| `/workspace` | Pipeline workspace directory |

## 📋 Requirements

- Docker with Linux container support
- Docker Desktop (Windows/macOS) with shared drives enabled

## ⚠️ Known Limitations (Alpha)

**Data Persistence:**
- All pipeline data is stored **in-memory only** — restarting the container clears all execution history, logs, and artifacts
- No database persistence between container restarts

**Pipeline Features Not Yet Supported:**
- Pipeline triggers (webhooks, scheduled, PR triggers) — manual execution only
- Template references from external repositories
- Variable groups and library/secure files
- Service connections
- Matrix and parallel job strategies
- Deployment jobs and environments
- Approvals and gates
- Classic release pipelines (YAML pipelines only)
- Container jobs with custom images (uses default Ubuntu-based agent)

**Task Support:**
- Limited to implemented tasks — unsupported tasks will be skipped or fail
- Some task inputs may not be fully implemented

**Other:**
- No authentication or multi-user support
- Single pipeline execution recommended at a time
- No integration with Azure DevOps organizations

## 📧 Support

For questions, feedback, or issues with this alpha release:
- **GitHub Issues**: [Report bugs or request features](https://github.com/jmkcoder/Azure-Tasks-Mock/issues)

---

*This is an alpha release. Features and APIs are subject to change.*
