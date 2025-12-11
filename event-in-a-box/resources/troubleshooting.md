# Troubleshooting Guide

This guide helps you diagnose and fix common issues when setting up and running agentic DevOps demos and workshops.

## Setup Issues

### Python Environment Problems

#### Issue: "Python version not supported"

```
Error: Python 3.7 is not supported. Please use Python 3.8 or later.
```

**Solution:**
```bash
# Check your Python version
python --version
python3 --version

# Install Python 3.8+ using package manager
# On macOS:
brew install python@3.11

# On Ubuntu:
sudo apt update
sudo apt install python3.11

# On Windows: Download from python.org
```

#### Issue: "Module not found" errors

```
ModuleNotFoundError: No module named 'requests'
```

**Solution:**
```bash
# Ensure you're in virtual environment
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows

# Reinstall requirements
pip install -r requirements.txt

# If issues persist, upgrade pip
pip install --upgrade pip
pip install -r requirements.txt --force-reinstall
```

#### Issue: "Permission denied" when installing packages

**Solution:**
```bash
# Use virtual environment (recommended)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Or install with --user flag
pip install --user -r requirements.txt
```

### GitHub Authentication Issues

#### Issue: "Bad credentials" or "401 Unauthorized"

**Solution:**
1. Verify your token has correct permissions:
   - `repo` (all)
   - `workflow`
   - `read:org` (if using org repos)

2. Check token is properly set:
```bash
# View current token (first few characters)
echo $GITHUB_TOKEN | cut -c1-10

# Re-set token
export GITHUB_TOKEN=your_token_here

# Or update .env file
echo "GITHUB_TOKEN=your_token_here" > .env
source .env
```

3. Regenerate token if expired:
   - Go to GitHub Settings → Developer settings → Personal access tokens
   - Delete old token
   - Generate new token with required scopes

#### Issue: "rate limit exceeded"

```
Error: API rate limit exceeded for user
```

**Solution:**
- Authenticated requests have higher limits
- Wait for rate limit reset (check `X-RateLimit-Reset` header)
- Use conditional requests with ETags
- Cache API responses when possible

```bash
# Check rate limit status
curl -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/rate_limit
```

### Docker Issues

#### Issue: "Cannot connect to Docker daemon"

```
Error: Cannot connect to the Docker daemon at unix:///var/run/docker.sock
```

**Solution:**
```bash
# Ensure Docker is running
docker ps

# On macOS: Start Docker Desktop
open -a Docker

# On Linux: Start Docker service
sudo systemctl start docker

# Check Docker status
sudo systemctl status docker
```

#### Issue: "Port already in use"

```
Error: bind: address already in use
```

**Solution:**
```bash
# Find process using the port
lsof -i :8080  # macOS/Linux
netstat -ano | findstr :8080  # Windows

# Kill the process
kill -9 <PID>  # macOS/Linux
taskkill /PID <PID> /F  # Windows

# Or use a different port
docker run -p 8081:8080 agentic-agent:latest
```

#### Issue: "Image not found"

```
Error: Unable to find image 'agentic-agent:latest' locally
```

**Solution:**
```bash
# Build the image first
docker build -t agentic-agent:latest .

# Or pull from registry
docker pull your-registry/agentic-agent:latest

# List available images
docker images
```

### Webhook Issues

#### Issue: Webhook not receiving events

**Symptoms:**
- Agent logs show no incoming requests
- GitHub webhook shows "Last delivery" but agent doesn't react

**Diagnosis:**
```bash
# Check webhook status in GitHub
gh api repos/{owner}/{repo}/hooks

# Check agent is listening
curl http://localhost:8080/health

# Check recent webhook deliveries
# Go to: Settings → Webhooks → Edit → Recent Deliveries
```

**Solutions:**

1. **Local development - use ngrok:**
```bash
# Install ngrok
brew install ngrok  # macOS
# Or download from ngrok.com

# Start ngrok tunnel
ngrok http 8080

# Update webhook URL in GitHub to ngrok URL
# Example: https://abc123.ngrok.io/webhook
```

2. **Firewall blocking:**
```bash
# Allow port through firewall (Linux)
sudo ufw allow 8080

# Check if port is accessible externally
curl http://your-external-ip:8080/health
```

3. **Wrong webhook secret:**
```bash
# Verify secret in .env matches GitHub webhook
# Update GitHub webhook secret to match .env
gh api repos/{owner}/{repo}/hooks/{hook_id} \
  -X PATCH \
  -f secret="your-secret"
```

#### Issue: Webhook receives events but agent doesn't process them

**Diagnosis:**
```bash
# Check agent logs
tail -f logs/agent.log

# Test webhook manually
curl -X POST http://localhost:8080/webhook \
  -H "Content-Type: application/json" \
  -H "X-GitHub-Event: workflow_run" \
  -d @test-payload.json
```

**Common causes:**
- Event type filtering too restrictive
- Error in event parsing
- Exception in event handler
- Missing event handlers

**Solution:**
```bash
# Enable debug logging
export LOG_LEVEL=DEBUG
python agent.py --mode service

# Add more event types to configuration
# Edit agent-config.yaml to include needed events
```

## Runtime Issues

### Agent Issues

#### Issue: Agent crashes immediately

**Diagnosis:**
```bash
# Run with verbose logging
python agent.py --mode service --log-level DEBUG

# Check for syntax errors
python -m py_compile agent.py

# Check dependencies
pip check
```

**Common causes:**
- Missing dependencies
- Configuration file errors
- Port conflicts
- Invalid credentials

#### Issue: Agent running but not responding to events

**Diagnosis:**
```bash
# Check if agent is actually running
ps aux | grep agent

# Check listening ports
netstat -an | grep 8080
lsof -i :8080

# Send test event
curl -X POST http://localhost:8080/webhook \
  -H "Content-Type: application/json" \
  -d '{"test": true}'

# Check response
curl http://localhost:8080/health
```

**Solutions:**
- Verify webhook configuration
- Check event filtering rules
- Review agent logs for errors
- Ensure event handlers are registered

#### Issue: Agent making wrong decisions

**Debugging steps:**

1. **Enable dry-run mode:**
```python
# In agent code
agent = Agent(dry_run=True)  # Logs actions without executing
```

2. **Review decision logic:**
```bash
# Add detailed logging
import logging
logging.basicConfig(level=logging.DEBUG)
```

3. **Test with known scenarios:**
```python
# Create test cases
def test_pattern_matching():
    agent = Agent()
    result = agent.match_pattern("error: module not found")
    assert result.pattern == "dependency_failure"
```

4. **Check pattern priorities:**
```yaml
# In agent-config.yaml, ensure patterns are ordered correctly
patterns:
  - name: specific_error      # More specific first
    priority: 10
  - name: general_error       # More general last
    priority: 1
```

### GitHub Actions Issues

#### Issue: Workflow not triggering

**Diagnosis:**
```bash
# Check workflow status
gh workflow list

# View workflow runs
gh run list --workflow=ci.yml

# Check workflow file syntax
yamllint .github/workflows/ci.yml
```

**Solutions:**

1. **Check trigger configuration:**
```yaml
# .github/workflows/ci.yml
on:
  push:
    branches: [main]  # Ensure correct branch
  workflow_dispatch:   # Allow manual trigger
```

2. **Trigger manually:**
```bash
gh workflow run ci.yml
```

3. **Check Actions are enabled:**
- Go to Settings → Actions → General
- Ensure "Allow all actions and reusable workflows" is selected

#### Issue: Workflow fails immediately

**Common causes:**
- Syntax errors in workflow file
- Missing secrets
- Invalid GitHub Actions expressions
- Wrong runner (e.g., using Windows commands on Linux)

**Solutions:**
```bash
# Validate workflow locally
act -l  # Lists workflows
act push  # Simulates push event

# Check secrets are set
gh secret list

# View workflow logs
gh run view --log
```

### Demo Issues

#### Issue: Demo fails during presentation

**Immediate recovery:**
1. **Switch to backup video/screenshots**
2. **Use pre-recorded demo**
3. **Walk through with slides/diagrams**
4. **Continue with next segment**

**Prevention:**
- Test entire demo flow before presentation
- Have backup video ready
- Use stable, known-good commits
- Have screenshots at key points
- Test in presentation environment

#### Issue: Audience can't follow along

**Solutions:**
- Increase terminal font size (minimum 18pt)
- Use high contrast color scheme
- Slow down and narrate actions
- Show overview slides between steps
- Use drawing tools to highlight
- Post command history in chat

#### Issue: Demo running slow

**Quick fixes:**
- Skip non-essential steps
- Use pre-built images instead of building
- Run on faster hardware
- Reduce data set size
- Cache dependencies
- Use local resources instead of remote

## Workshop Issues

### Participant Setup Issues

#### Issue: Participants can't install prerequisites

**Solutions:**
- Provide pre-configured cloud environments (GitHub Codespaces, Gitpod)
- Use Docker containers for consistency
- Have installation help desk during setup time
- Pair participants (working setup helps those without)
- Provide offline installers on USB drives

#### Issue: Varied experience levels

**Strategies:**
- Provide multiple difficulty tracks
- Use pair programming
- Create challenge extensions for fast learners
- Offer simplified alternatives for strugglers
- Have TAs available for individual help

#### Issue: Time running over

**Adjustments:**
- Skip optional sections
- Reduce Q&A time (save for after)
- Streamline demonstrations
- Provide pre-built solutions
- Assign remaining work as homework

### Technical Issues During Workshop

#### Issue: WiFi/Internet problems

**Mitigations:**
- Have offline documentation
- Pre-download all dependencies
- Use local resources
- Provide USB with materials
- Use mobile hotspot as backup

#### Issue: GitHub rate limiting affecting multiple participants

**Solutions:**
```bash
# Use GitHub Enterprise or team account
# Rotate through multiple tokens
# Cache responses locally
# Use local GitLab/Gitea instance

# Check rate limit
gh api rate_limit
```

#### Issue: Everyone's agents interfering with each other

**Solutions:**
- Use separate repositories per participant
- Use branches instead of separate repos
- Add participant ID to agent names
- Use different webhooks per participant
- Implement agent isolation in config

## Performance Issues

### Agent Performance

#### Issue: Agent response time too slow

**Profiling:**
```python
# Add timing decorator
import time
from functools import wraps

def timing(f):
    @wraps(f)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = f(*args, **kwargs)
        end = time.time()
        print(f'{f.__name__} took {end-start:.2f}s')
        return result
    return wrapper

@timing
def process_event(event):
    # ... agent logic ...
```

**Optimizations:**
- Cache frequently accessed data
- Use async operations
- Optimize API calls (batch requests)
- Improve pattern matching algorithms
- Add request timeout limits
- Scale horizontally

#### Issue: High memory usage

**Diagnosis:**
```python
# Memory profiling
from memory_profiler import profile

@profile
def memory_intensive_function():
    # ... code ...
```

**Solutions:**
- Stream large datasets instead of loading into memory
- Clear caches periodically
- Use generators instead of lists
- Profile and fix memory leaks
- Increase available memory

### GitHub Actions Performance

#### Issue: Workflows taking too long

**Optimizations:**
- Cache dependencies
- Use matrix strategy for parallel execution
- Optimize test suite
- Use faster runners
- Split into multiple workflows

```yaml
# Use caching
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

# Use matrix for parallel runs
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node: [16, 18, 20]
```

## Getting Help

### Before Asking for Help

Gather this information:
- [ ] Exact error message
- [ ] Steps to reproduce
- [ ] Environment details (OS, versions)
- [ ] Relevant logs
- [ ] What you've tried already

### Where to Get Help

1. **Documentation:**
   - [README](../README.md)
   - [FAQ](./faq.md)
   - [Setup Guide](../setup/demo-setup.md)

2. **Community:**
   - GitHub Discussions
   - Slack/Discord channel
   - Stack Overflow (tag: agentic-devops)

3. **Support:**
   - Open GitHub Issue
   - Email support
   - Schedule office hours

### Reporting Bugs

Include:
```markdown
**Description:** Brief description of issue

**Steps to Reproduce:**
1. Step one
2. Step two
3. ...

**Expected Behavior:** What should happen

**Actual Behavior:** What actually happens

**Environment:**
- OS: [e.g., macOS 12.0]
- Python version: [e.g., 3.11.0]
- Agent version: [e.g., 1.2.3]
- Docker version: [if applicable]

**Logs:**
```bash
# Include relevant logs
```

**Screenshots:** [if applicable]
```

## Emergency Contacts

During live events:
- **Technical Lead:** [Contact info]
- **Backup Presenter:** [Contact info]
- **IT Support:** [Contact info]
- **Venue Contact:** [Contact info]

## Appendix: Common Error Messages

### "Connection refused"
- Service not running
- Wrong host/port
- Firewall blocking
- Network issue

### "Permission denied"
- Insufficient permissions
- Need sudo/admin rights
- File ownership issue
- Token lacks required scopes

### "Timeout"
- Network issue
- Service overloaded
- Request taking too long
- Firewall/proxy blocking

### "Invalid configuration"
- Syntax error in config file
- Missing required field
- Invalid value
- Version mismatch
