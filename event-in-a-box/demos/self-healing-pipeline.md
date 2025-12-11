# Demo: Self-Healing CI/CD Pipeline

## Overview

This demo showcases an intelligent agent that monitors a CI/CD pipeline and automatically fixes common failures without human intervention.

**Duration:** 5-10 minutes  
**Difficulty:** Beginner  
**Best for:** Introduction to agentic DevOps concepts

## What You'll Demonstrate

1. A CI/CD pipeline that encounters a common failure
2. An agent detecting the failure automatically
3. The agent analyzing the failure pattern
4. Automatic remediation being applied
5. Pipeline recovery and success

## Prerequisites

- GitHub repository with Actions enabled
- Python 3.8+
- GitHub CLI (`gh`) installed and authenticated
- Docker (optional, for local testing)

## Setup (15 minutes before demo)

### 1. Prepare the Repository

```bash
# Clone the demo repository
git clone https://github.com/[your-org]/agentic-pipeline-demo
cd agentic-pipeline-demo

# Install dependencies
pip install -r requirements.txt

# Configure the agent
cp .env.example .env
# Edit .env with your GitHub token
```

### 2. Configure the Agent

The agent monitors for these failure patterns:
- Dependency installation failures
- Test environment setup issues
- Flaky test failures
- Resource timeout errors

Configuration file: `agent-config.yaml`

```yaml
agent:
  name: pipeline-healer
  triggers:
    - event: workflow_run
      status: failure
  patterns:
    - name: dependency_failure
      match: "Could not find package"
      action: retry_with_cache_clear
    - name: flaky_test
      match: "Test failed intermittently"
      action: retry_test_suite
  max_retries: 3
  notification:
    on_success: true
    on_failure: true
```

### 3. Test the Environment

```bash
# Test agent connectivity
python agent.py --test

# Verify GitHub Actions access
gh workflow list

# Run a test cycle
python agent.py --dry-run
```

## Demo Script

### Part 1: Introduction (1 minute)

**What to say:**
> "Today we're going to see how an intelligent agent can automatically fix common CI/CD pipeline failures. This is a real GitHub Actions workflow, and we're going to trigger a failure that the agent will detect and fix without any human intervention."

**What to show:**
- Open the GitHub repository in your browser
- Show the Actions tab
- Highlight recent successful runs

### Part 2: Trigger a Failure (2 minutes)

**What to do:**
```bash
# Create a branch with a known issue
git checkout -b demo-failure
echo "import nonexistent_package" >> src/app.py
git add .
git commit -m "Introduce dependency issue"
git push origin demo-failure
```

**What to say:**
> "I'm introducing a common issue - a missing package import. This happens all the time when dependencies aren't properly specified. Let's watch what happens."

**What to show:**
- Show the file change in VS Code or GitHub
- Navigate to Actions tab
- Show the workflow starting

### Part 3: Agent Detection (2 minutes)

**What to show:**
- Switch to terminal showing agent logs
- Point out the detection message

```
[Agent] Detected workflow_run event: run_12345
[Agent] Status: failure
[Agent] Analyzing logs...
[Agent] Pattern matched: dependency_failure
[Agent] Confidence: 95%
[Agent] Initiating remediation...
```

**What to say:**
> "The agent detected the failure immediately. It analyzed the error logs, matched it against known patterns, and identified this as a dependency issue with high confidence. Now it's going to take action."

### Part 4: Automatic Remediation (2 minutes)

**What to show:**
- Agent logs showing remediation steps

```
[Agent] Action: retry_with_cache_clear
[Agent] Clearing dependency cache...
[Agent] Updating requirements.txt...
[Agent] Triggering retry...
[Agent] New run: run_12346
```

**What to say:**
> "The agent cleared the cache, verified the requirements file, and triggered a new run. This all happened in seconds, without anyone having to investigate the logs or manually retry the workflow."

### Part 5: Success and Learning (1 minute)

**What to show:**
- GitHub Actions showing the new run succeeding
- Agent logs showing success

```
[Agent] Run run_12346: success
[Agent] Resolution time: 45 seconds
[Agent] Updating knowledge base...
[Agent] Similar issues in history: 3
[Agent] Success rate: 100%
```

**What to say:**
> "Success! The pipeline is now passing. The agent also updated its knowledge base, so it will handle similar issues even faster next time. Notice the resolution time - 45 seconds from failure to fix. Without automation, this could have taken minutes or hours depending on when someone noticed."

### Part 6: Show the Agent Dashboard (Optional, 1 minute)

**What to show:**
- Agent dashboard or metrics page
- Historical success rate
- Common failure patterns
- Time savings metrics

**What to say:**
> "Here's the agent's dashboard showing all the incidents it's handled. You can see patterns, success rates, and how much time it's saved the team."

## Key Points to Emphasize

1. **Speed:** From failure to resolution in under a minute
2. **Autonomy:** No human intervention required
3. **Learning:** Agent improves over time
4. **Consistency:** Same issue handled the same way every time
5. **Transparency:** Complete audit trail of all actions

## Common Questions

**Q: What if the agent makes the wrong decision?**
A: The agent operates within defined guardrails. For high-risk actions, it can require human approval. All actions are logged and can be reviewed or reversed.

**Q: How does it learn?**
A: It builds a knowledge base of patterns and outcomes. Over time, it recognizes similar issues and improves its confidence and speed.

**Q: Can it handle any failure?**
A: It handles common, well-understood failures automatically. Novel or critical failures are escalated to humans with all the diagnostic data already collected.

**Q: Is this production-ready?**
A: Yes! Many teams use similar approaches. Start with low-risk scenarios and expand as you build confidence.

## Backup Plan

If the live demo fails:

1. **Have a video recording** of the successful demo
2. **Use screenshots** at key stages
3. **Show historical runs** in GitHub Actions
4. **Walk through the code** and explain the logic

## Variations

### Shorter Version (3 minutes)
- Skip the setup explanation
- Use a pre-recorded failure
- Focus on agent response only

### Longer Version (15 minutes)
- Show the agent code
- Explain the pattern matching logic
- Configure a new pattern live
- Demonstrate notification integration

### Workshop Version
- Have participants trigger their own failures
- Let them modify agent configuration
- Group exercise: identify patterns to automate

## Cleanup

After the demo:

```bash
# Delete the demo branch
git push origin --delete demo-failure
git branch -D demo-failure

# Reset agent state (if needed)
python agent.py --reset
```

## Files Needed

- `agent.py` - The agent implementation
- `agent-config.yaml` - Agent configuration
- `.github/workflows/ci.yml` - The CI workflow
- `requirements.txt` - Python dependencies
- `src/app.py` - Sample application

## Additional Resources

- Agent architecture diagram
- Configuration reference guide
- API documentation
- Troubleshooting guide
