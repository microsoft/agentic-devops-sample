# Workshop Materials

## Overview

This directory contains hands-on exercises and lab materials for the Agentic DevOps workshop.

## Workshop Structure

### Duration Options

- **Half-day (4 hours)**: Core concepts + 2 labs
- **Full-day (8 hours)**: All concepts + 4 labs + project
- **Multi-day**: Deep dive with advanced topics

## Lab Exercises

### Lab 1: Build Your First Agent (60 minutes)
**Difficulty:** Beginner  
**File:** [lab1-first-agent.md](./lab1-first-agent.md)

Create a simple monitoring agent that responds to GitHub webhook events.

**Learning Objectives:**
- Understand agent architecture
- Handle webhook events
- Implement basic decision logic
- Execute automated actions

### Lab 2: Pattern Matching and Response (60 minutes)
**Difficulty:** Beginner-Intermediate  
**File:** [lab2-pattern-matching.md](./lab2-pattern-matching.md)

Build an agent that recognizes error patterns and applies appropriate fixes.

**Learning Objectives:**
- Pattern recognition techniques
- Regular expressions for log analysis
- Decision trees
- Action mapping

### Lab 3: Self-Healing Pipeline (90 minutes)
**Difficulty:** Intermediate  
**File:** [lab3-self-healing.md](./lab3-self-healing.md)

Implement a complete self-healing CI/CD pipeline with multiple recovery strategies.

**Learning Objectives:**
- Failure detection
- Root cause analysis
- Remediation strategies
- Rollback mechanisms

### Lab 4: Multi-Agent System (90 minutes)
**Difficulty:** Advanced  
**File:** [lab4-multi-agent.md](./lab4-multi-agent.md)

Create a system with multiple specialized agents working together.

**Learning Objectives:**
- Agent coordination
- Message passing
- Shared state management
- Conflict resolution

## Prerequisites

### For Participants

**Required Knowledge:**
- Basic programming (Python or JavaScript)
- Git fundamentals
- Command line basics
- Understanding of CI/CD concepts

**Required Setup:**
- Laptop with development environment
- GitHub account
- Git installed
- Python 3.8+ or Node.js 16+
- Text editor or IDE
- Docker Desktop (for advanced labs)

### For Instructors

**Preparation Time:** 2-3 hours before workshop

**Required:**
- Complete all labs yourself
- Test all code samples
- Prepare demo repository
- Set up participant accounts/access
- Verify all links and resources
- Prepare troubleshooting guide
- Have backup materials ready

## Workshop Schedule

### Half-Day Format (4 hours)

```
09:00 - 09:15 | Welcome & Introduction
09:15 - 10:00 | Presentation: Agentic DevOps Fundamentals
10:00 - 10:15 | Break
10:15 - 11:15 | Lab 1: Build Your First Agent
11:15 - 11:30 | Break
11:30 - 12:30 | Lab 2: Pattern Matching
12:30 - 13:00 | Q&A and Wrap-up
```

### Full-Day Format (8 hours)

```
09:00 - 09:30 | Welcome & Introduction
09:30 - 10:30 | Presentation: Agentic DevOps
10:30 - 10:45 | Break
10:45 - 11:45 | Lab 1: Build Your First Agent
11:45 - 13:00 | Lunch
13:00 - 14:00 | Lab 2: Pattern Matching
14:00 - 14:15 | Break
14:15 - 15:45 | Lab 3: Self-Healing Pipeline
15:45 - 16:00 | Break
16:00 - 17:00 | Lab 4: Multi-Agent System
17:00 - 17:30 | Showcase & Wrap-up
```

## Instructor Guide

### Before the Workshop

1. **Setup Checklist** (1 week before)
   - [ ] Reserve venue/virtual space
   - [ ] Send calendar invites
   - [ ] Share prerequisite installation guide
   - [ ] Prepare GitHub organization/repositories
   - [ ] Test all lab materials
   - [ ] Print/share handouts

2. **Technical Preparation** (1 day before)
   - [ ] Test WiFi/connectivity
   - [ ] Verify projection/screen sharing
   - [ ] Clone repositories for participants
   - [ ] Prepare backup USB drives
   - [ ] Test all demo scripts
   - [ ] Have offline documentation ready

3. **Day of Workshop**
   - [ ] Arrive early for setup
   - [ ] Test AV equipment
   - [ ] Have contact info displayed
   - [ ] Prepare ice breaker activity
   - [ ] Review emergency contacts

### During the Workshop

**Pacing Tips:**
- Start with a quick poll of experience levels
- Adjust depth based on audience
- Use timers for labs
- Walk around to help participants
- Identify common issues early
- Pair up struggling participants with others
- Keep energy high with breaks and movement

**Engagement Strategies:**
- Ask questions frequently
- Encourage pair programming
- Create friendly competition
- Share real-world stories
- Use humor appropriately
- Celebrate small wins

**Handling Questions:**
- Answer briefly, offer to discuss details in breaks
- Use "parking lot" for off-topic questions
- Turn questions into learning opportunities
- Don't fake knowledge - it's OK to say "I don't know"

### After the Workshop

1. **Immediate Follow-up**
   - Share slides and materials
   - Send feedback survey
   - Answer outstanding questions
   - Share photos (with permission)

2. **Within a Week**
   - Review feedback
   - Update materials based on feedback
   - Send additional resources
   - Offer office hours for questions

3. **Long-term**
   - Stay connected via community channels
   - Share new resources
   - Track participant success stories
   - Iterate on materials

## Lab Materials

### What's Provided

Each lab includes:
- **Instructions**: Step-by-step guide
- **Starter Code**: Template to begin with
- **Solution Code**: Reference implementation
- **Test Cases**: Validation scripts
- **Troubleshooting**: Common issues and fixes

### Repository Structure

```
workshop/
├── lab1-first-agent/
│   ├── README.md
│   ├── starter/
│   ├── solution/
│   └── tests/
├── lab2-pattern-matching/
│   ├── README.md
│   ├── starter/
│   ├── solution/
│   └── tests/
├── lab3-self-healing/
│   ├── README.md
│   ├── starter/
│   ├── solution/
│   └── tests/
└── lab4-multi-agent/
    ├── README.md
    ├── starter/
    ├── solution/
    └── tests/
```

## Assessment and Certification

### Lab Completion Criteria

Each lab must demonstrate:
- [ ] Functional code that meets requirements
- [ ] Proper error handling
- [ ] Tests passing
- [ ] Documentation complete

### Optional Certification

Participants who complete all labs and pass the final assessment can receive:
- Certificate of completion
- Digital badge
- LinkedIn endorsement
- Access to advanced materials

## Troubleshooting Guide

### Common Participant Issues

**"My agent isn't receiving webhooks"**
- Check webhook configuration
- Verify URL is accessible
- Check firewall/network settings
- Try ngrok or similar tunneling tool

**"The code doesn't work on my machine"**
- Verify all prerequisites installed
- Check Python/Node version
- Review environment variables
- Try the Docker alternative

**"I'm stuck on step X"**
- Pair with another participant
- Review solution hints
- Ask instructor for help
- Skip to next checkpoint

**"This is too fast/slow"**
- Provide optional challenges for fast learners
- Offer simplified paths for struggling participants
- Use flexible pacing
- Have extension activities ready

## Adaptations

### For Different Audiences

**Executives/Non-Technical:**
- Focus on concepts over coding
- Use no-code tools
- Emphasize business value
- Include more demos, fewer labs

**Experienced Engineers:**
- Move faster through basics
- Add advanced challenges
- Discuss architecture tradeoffs
- Encourage experimentation

**Mixed Experience Levels:**
- Pair experienced with beginners
- Offer multiple difficulty paths
- Provide extension challenges
- Use differentiated instruction

### For Virtual Delivery

**Adjustments:**
- Shorten sessions (max 90 min blocks)
- More frequent breaks
- Use breakout rooms for labs
- Increase interaction
- Record sessions for later viewing
- Provide better written instructions

**Tools:**
- Zoom/Teams for video
- Slack/Discord for chat
- GitHub Codespaces for uniform environments
- Miro/Mural for collaboration
- Polling tools for engagement

## Success Metrics

### For Participants

- % completing all labs
- Average assessment scores
- Post-workshop survey results
- Implementation rate (did they use it?)

### For Organization

- ROI in automation time saved
- Incidents prevented/resolved
- Team satisfaction improvement
- Adoption rate

## Additional Resources

- [Pre-Workshop Setup Guide](./pre-workshop-setup.md)
- [Instructor Tips & Tricks](./instructor-guide.md)
- [Participant Handbook](./participant-handbook.md)
- [Troubleshooting Database](./troubleshooting.md)

## Feedback and Improvement

After each workshop, please document:
- What went well
- What needs improvement
- Common stumbling blocks
- New questions that arose
- Ideas for new content

Share feedback via:
- GitHub Issues
- Community Forums
- Direct contact with maintainers
