# Demo Environment Setup Guide

This guide will help you set up everything you need to deliver the agentic DevOps demos.

## Overview

The demo environment includes:
- GitHub repository with sample workflows
- Agent monitoring service
- Demo applications
- Dashboard for visualizing agent activity

## Time Required

- Initial setup: 30-45 minutes
- Pre-demo verification: 10-15 minutes

## Prerequisites

### Required
- GitHub account with organization access
- Admin permissions on a GitHub repository
- Terminal/command line access
- Git installed
- Internet connection

### Optional (depending on demos)
- Docker Desktop
- Python 3.8 or later
- Node.js 16 or later
- Azure or AWS account (for cloud demos)
- kubectl (for Kubernetes demos)

## Step-by-Step Setup

### 1. Create Demo Repository

```bash
# Clone the template
gh repo create agentic-devops-demo --template microsoft/agentic-devops-sample --public

# Navigate to the repository
cd agentic-devops-demo
```

### 2. Configure GitHub Actions

Enable GitHub Actions in your repository:
1. Go to Settings → Actions → General
2. Set "Actions permissions" to "Allow all actions"
3. Enable "Read and write permissions" for workflows

### 3. Install Required Tools

#### Python Environment

```bash
# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Verify installation
python agent.py --version
```

#### GitHub CLI

```bash
# Install gh (if not already installed)
# On macOS:
brew install gh

# On Windows:
winget install GitHub.cli

# On Linux:
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Authenticate
gh auth login
```

#### Docker (Optional)

```bash
# Install Docker Desktop from https://www.docker.com/products/docker-desktop

# Verify installation
docker --version
docker compose --version
```

### 4. Configure Secrets and Tokens

#### Create GitHub Personal Access Token

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Select scopes:
   - `repo` (all)
   - `workflow`
   - `read:org`
4. Copy the token

#### Set Environment Variables

```bash
# Create .env file
cp .env.example .env

# Edit .env with your values
cat > .env << EOF
GITHUB_TOKEN=your_token_here
GITHUB_REPO=your-org/agentic-devops-demo
AGENT_NAME=demo-agent
LOG_LEVEL=INFO
EOF

# Source the environment variables
source .env
```

#### Configure Repository Secrets

```bash
# Using GitHub CLI
gh secret set GITHUB_TOKEN --body "$GITHUB_TOKEN"

# Or manually in GitHub:
# Settings → Secrets and variables → Actions → New repository secret
```

### 5. Deploy the Agent

#### Local Deployment

```bash
# Start the agent service
python agent.py --mode service

# In a new terminal, verify it's running
curl http://localhost:8080/health
```

#### Docker Deployment

```bash
# Build the agent container
docker build -t agentic-agent:latest .

# Run the agent
docker run -d \
  --name agentic-agent \
  -p 8080:8080 \
  --env-file .env \
  agentic-agent:latest

# Check logs
docker logs -f agentic-agent
```

#### Cloud Deployment (Optional)

For Azure Container Instances:

```bash
# Login to Azure
az login

# Create resource group
az group create --name agentic-demo-rg --location eastus

# Deploy container
az container create \
  --resource-group agentic-demo-rg \
  --name agentic-agent \
  --image agentic-agent:latest \
  --environment-variables \
    GITHUB_TOKEN=$GITHUB_TOKEN \
    GITHUB_REPO=$GITHUB_REPO \
  --ports 8080
```

### 6. Configure Webhooks

#### Automated Setup

```bash
# Create webhook using script
python scripts/setup-webhook.py

# Verify webhook
gh api repos/{owner}/{repo}/hooks
```

#### Manual Setup

1. Go to repository Settings → Webhooks → Add webhook
2. Payload URL: `http://your-agent-url:8080/webhook`
3. Content type: `application/json`
4. Select events:
   - Workflow runs
   - Pull requests
   - Issues
5. Click "Add webhook"

### 7. Verify Setup

Run the verification script:

```bash
# Run full verification
python scripts/verify-setup.py

# Expected output:
# ✓ GitHub authentication successful
# ✓ Repository accessible
# ✓ GitHub Actions enabled
# ✓ Agent service running
# ✓ Webhook configured
# ✓ Demo workflows present
# ✓ All checks passed!
```

### 8. Run a Test Workflow

```bash
# Trigger a test workflow
gh workflow run test-agent.yml

# Watch the run
gh run watch

# Check agent logs
python agent.py --logs --tail 50
```

## Pre-Demo Checklist

Run this checklist 10-15 minutes before your presentation:

```bash
# Run the pre-demo check script
python scripts/pre-demo-check.py
```

Manual checks:
- [ ] Agent service is running
- [ ] Webhook is receiving events
- [ ] GitHub Actions has sufficient minutes
- [ ] All secrets are configured
- [ ] Demo branches are clean
- [ ] Terminal font size is readable (18pt+)
- [ ] Browser tabs are prepared
- [ ] Backup video/screenshots ready
- [ ] Internet connection stable

## Troubleshooting

### Agent Not Starting

```bash
# Check Python version
python --version  # Should be 3.8+

# Check dependencies
pip list

# Reinstall dependencies
pip install --upgrade -r requirements.txt

# Check for port conflicts
lsof -i :8080  # On macOS/Linux
netstat -ano | findstr :8080  # On Windows
```

### Webhook Not Receiving Events

```bash
# Check webhook deliveries in GitHub
# Settings → Webhooks → Edit → Recent Deliveries

# Check agent logs
tail -f logs/agent.log

# Test webhook manually
curl -X POST http://localhost:8080/webhook \
  -H "Content-Type: application/json" \
  -d '{"test": true}'
```

### GitHub Actions Not Triggering

```bash
# Check workflow files
gh workflow list

# View workflow runs
gh run list

# Check repository permissions
gh api repos/{owner}/{repo} --jq .permissions
```

### Docker Issues

```bash
# Check Docker is running
docker ps

# View container logs
docker logs agentic-agent

# Restart container
docker restart agentic-agent

# Rebuild if needed
docker build --no-cache -t agentic-agent:latest .
```

## Quick Reset

If something goes wrong during setup:

```bash
# Stop all services
python scripts/stop-all.py

# Clean up
rm -rf logs/
rm -rf __pycache__/
docker stop agentic-agent
docker rm agentic-agent

# Start fresh
python scripts/setup-demo.py --fresh
```

## Environment Configurations

### Minimal (for basic demos)
- GitHub repository
- Local Python agent
- No Docker required

### Standard (recommended)
- GitHub repository
- Dockerized agent
- Local dashboard

### Full (for workshops)
- GitHub repository
- Cloud-hosted agent
- Dashboard with metrics
- Multiple demo apps

## Resource Requirements

### Local Development
- CPU: 2+ cores
- RAM: 4GB+
- Disk: 10GB free space
- Network: Stable internet connection

### Cloud Deployment
- Azure: B1s or larger
- AWS: t3.small or larger
- GCP: e2-small or larger

## Costs

- GitHub: Free for public repositories
- Docker: Free for personal use
- Cloud hosting: ~$5-10/month (if running 24/7)

**Demo tip:** Use cloud free tiers or shut down resources between demos to minimize costs.

## Additional Resources

- [Agent Configuration Reference](../resources/agent-config.md)
- [Troubleshooting Guide](../resources/troubleshooting.md)
- [Demo Scripts](../demos/README.md)
- [FAQ](../resources/faq.md)

## Getting Help

If you encounter issues during setup:
1. Check the [Troubleshooting Guide](../resources/troubleshooting.md)
2. Review [GitHub Discussions](https://github.com/microsoft/agentic-devops-sample/discussions)
3. Open an issue with setup logs
4. Ask in the community Slack/Discord

## Next Steps

Once setup is complete:
1. Review the [Speaker Guide](../speaker-guide.md)
2. Practice with the [Demo Scripts](../demos/README.md)
3. Customize the [Presentation](../presentation/slides.md)
4. Run through at least one full demo
