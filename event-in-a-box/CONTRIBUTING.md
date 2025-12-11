# Contributing to Event in a Box

Thank you for your interest in contributing to the Agentic DevOps Event in a Box! This guide will help you get started.

## How to Contribute

There are many ways to contribute:

### 📝 Content Contributions
- Improve documentation
- Add new demo scripts
- Create workshop exercises
- Write blog posts or tutorials
- Translate content

### 🐛 Bug Reports
- Report issues with setup instructions
- Document problems with demos
- Identify outdated content

### 💡 Feature Requests
- Suggest new demos
- Propose new workshop modules
- Request additional resources

### 🎨 Design Improvements
- Improve slide templates
- Create diagrams and visualizations
- Design promotional materials

### 🧪 Testing
- Test demos in different environments
- Validate workshop materials
- Provide feedback on content

## Getting Started

### 1. Fork the Repository

```bash
# Fork on GitHub, then clone your fork
git clone https://github.com/YOUR-USERNAME/agentic-devops-sample
cd agentic-devops-sample
```

### 2. Create a Branch

```bash
# Create a descriptive branch name
git checkout -b feature/add-kubernetes-demo
# or
git checkout -b fix/update-setup-instructions
# or
git checkout -b docs/improve-speaker-guide
```

### 3. Make Your Changes

Follow these guidelines:

#### Documentation
- Use clear, concise language
- Include examples where helpful
- Test all commands and code snippets
- Check spelling and grammar
- Follow existing formatting style

#### Demo Scripts
- Include prerequisites
- Provide step-by-step instructions
- Add troubleshooting tips
- Test in clean environment
- Include expected output

#### Code
- Follow existing code style
- Add comments for complex logic
- Include error handling
- Write tests if applicable
- Update documentation

### 4. Test Your Changes

```bash
# Test documentation links
markdown-link-check **/*.md

# Test demo scripts
cd event-in-a-box/demos
./test-all-demos.sh

# Spell check
aspell check file.md
```

### 5. Commit Your Changes

Write clear, descriptive commit messages:

```bash
# Good commit messages
git commit -m "Add Kubernetes demo script with troubleshooting section"
git commit -m "Fix broken link in setup guide"
git commit -m "Update Python requirements for security patches"

# Less helpful commit messages (avoid these)
git commit -m "Update docs"
git commit -m "Fix stuff"
git commit -m "WIP"
```

### 6. Push and Create Pull Request

```bash
# Push to your fork
git push origin feature/add-kubernetes-demo

# Create pull request on GitHub
# Provide clear description of changes
```

## Pull Request Guidelines

### PR Description Template

```markdown
## Description
Brief description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Content improvement
- [ ] Other (please describe)

## Changes Made
- Bullet point list of specific changes

## Testing Done
- How you tested these changes
- Environments tested in

## Screenshots (if applicable)
Add screenshots to help explain your changes

## Checklist
- [ ] I have tested these changes
- [ ] Documentation is updated
- [ ] All links work
- [ ] Code follows style guidelines
- [ ] Commit messages are clear
```

### Review Process

1. **Automated Checks**: CI will run automatically
2. **Maintainer Review**: A maintainer will review your PR
3. **Feedback**: Address any requested changes
4. **Approval**: Once approved, your PR will be merged

## Content Guidelines

### Writing Style

- **Be Clear**: Use simple, straightforward language
- **Be Concise**: Get to the point quickly
- **Be Practical**: Include real examples
- **Be Inclusive**: Use inclusive language
- **Be Accurate**: Test everything you document

### Formatting Standards

#### Markdown
```markdown
# Main Title (H1)

## Section (H2)

### Subsection (H3)

**Bold** for emphasis
*Italic* for slight emphasis
`code` for inline code
```

#### Code Blocks
```markdown
```bash
# Always specify language
# Include comments
command --with-flags
```
```

#### Links
```markdown
[Descriptive Text](./relative/path/to/file.md)
[External Link](https://example.com)
```

### Demo Script Standards

Every demo should include:

1. **Overview**: What it demonstrates
2. **Duration**: How long it takes
3. **Difficulty**: Beginner/Intermediate/Advanced
4. **Prerequisites**: What's needed
5. **Setup**: Preparation steps
6. **Script**: Step-by-step instructions
7. **Common Issues**: Troubleshooting
8. **Cleanup**: How to reset

### Workshop Material Standards

Every lab should include:

1. **Learning Objectives**: What participants will learn
2. **Prerequisites**: Required knowledge
3. **Instructions**: Clear steps
4. **Starter Code**: Template to begin
5. **Solution**: Reference implementation
6. **Tests**: Validation
7. **Extensions**: Optional challenges

## Code of Conduct

### Our Standards

- **Be Respectful**: Treat everyone with respect
- **Be Collaborative**: Work together constructively
- **Be Inclusive**: Welcome diverse perspectives
- **Be Patient**: Everyone is learning
- **Be Professional**: Keep discussions on-topic

### Unacceptable Behavior

- Harassment or discrimination
- Trolling or insulting comments
- Personal attacks
- Publishing private information
- Other unprofessional conduct

## Recognition

Contributors will be recognized in:
- README contributors section
- Release notes
- Community highlights
- Speaker acknowledgments (if you present using these materials)

## Questions?

- 💬 **Discussions**: Use GitHub Discussions for questions
- 📧 **Email**: [maintainer-email]
- 💡 **Issues**: Open an issue for bugs or features
- 📣 **Slack/Discord**: Join the community channel

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Thank You!

Your contributions help make this resource better for everyone. We appreciate your time and effort! 🎉
