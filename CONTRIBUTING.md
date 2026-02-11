# Contributing to Greet Plugin

Thank you for your interest in contributing to the Greet Plugin! This document provides guidelines and instructions for contributing.

## Code of Conduct

- Be respectful and inclusive
- Welcome new contributors
- Focus on constructive feedback
- Report issues responsibly

## Getting Started

### Prerequisites

- Claude Code v2.1.39 or later
- Basic understanding of Claude plugins
- Familiarity with YAML and Markdown

### Local Development

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/greet-plugin.git
cd greet-plugin
```

2. **Enable locally:**
```bash
# Copy to local plugins directory
cp -r . ~/.claude/plugins/my-plugins/greet-plugin

# Update settings.json to point to your fork
```

3. **Test changes:**
```bash
/greet torvalds
/greet Sophia
/greet test-error-handling
```

## Types of Contributions

### Bug Reports
Found a bug? Please create a GitHub issue with:
- **Title**: Brief description of the bug
- **Environment**: Claude Code version, OS, plugin version
- **Steps to reproduce**: Exact steps to trigger the bug
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happened
- **Screenshots/logs**: Any relevant error messages

### Feature Requests
Have an idea? Create a GitHub Discussion or issue with:
- **Feature description**: What should the feature do?
- **Use case**: Why would this be useful?
- **Implementation approach** (optional): How might it work?
- **Alternative approaches** (optional): Other ways to solve it?

### Code Contributions

#### Before You Start
1. Check existing issues/PRs to avoid duplicates
2. Create an issue discussing the change first
3. Wait for approval before significant work
4. Follow the coding standards below

#### Coding Standards

**Agent Files (.md)**
- Clear, concise instructions
- Error handling documented
- Tool usage explicit
- Examples provided

**Command Files (.md)**
- Step-by-step workflow documented
- Error handling for all branches
- Fallback behaviors specified
- Performance considerations noted

**Configuration Files (.json)**
- Valid JSON (use validator)
- Consistent formatting
- Comments for complex settings

**Documentation**
- Clear and concise
- Examples for features
- Links to related docs
- Up-to-date information

#### Commit Messages

Follow conventional commits format:

```
type(scope): subject

body (optional)

footer (optional)
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `refactor:` Code refactoring
- `docs:` Documentation update
- `test:` Test additions/updates
- `perf:` Performance improvement
- `chore:` Build, CI, dependency updates

**Examples:**
```
feat(agent): add timeout handling to github-profile-analyst

- Add 5-second timeout for GitHub API calls
- Implement fallback to cached data
- Log timeout events for monitoring

Fixes #42
```

```
fix(command): correct input detection for email addresses

- Email addresses now properly recognized
- Extract name from email for greeting
- Add test cases for email inputs
```

#### Pull Request Process

1. **Create a feature branch:**
```bash
git checkout -b feature/your-feature-name
```

2. **Make your changes:**
   - Update relevant files
   - Add documentation
   - Test thoroughly

3. **Commit with meaningful messages:**
```bash
git add .
git commit -m "feat(scope): description"
```

4. **Push to your fork:**
```bash
git push origin feature/your-feature-name
```

5. **Create a Pull Request:**
   - Link related issues
   - Describe changes and rationale
   - Include before/after behavior
   - Note any breaking changes

6. **Respond to review feedback:**
   - Request changes as needed
   - Re-test after changes
   - Maintain conversational tone

### Documentation Contributions

**Improving README.md:**
- Fix typos/clarity
- Add examples
- Improve instructions
- Update outdated info

**Adding guides:**
- Create new .md files for complex topics
- Link from README.md
- Include examples
- Keep concise

**Improving inline docs:**
- Add comments to complex logic
- Clarify agent instructions
- Document error handling

## Testing

### What to Test

Before submitting a PR, test:

1. **Happy path:**
   - `/greet torvalds` (GitHub user)
   - `/greet Sophia` (regular name)
   - `/greet alice@example.com` (email)

2. **Error handling:**
   - Invalid input
   - Network timeout
   - API unavailable
   - Rate limiting

3. **Edge cases:**
   - Very long names
   - Special characters
   - Numbers in input
   - Multiple spaces

4. **Performance:**
   - Response time
   - Parallel execution
   - Caching behavior

### Test Checklist

Before submitting PR:
- [ ] Plugin enables correctly
- [ ] Agents execute without errors
- [ ] GitHub MCP integration works
- [ ] Fallback behavior works
- [ ] Error handling tested
- [ ] Documentation accurate
- [ ] No breaking changes
- [ ] Performance acceptable

## Review Process

### What Reviewers Look For

1. **Code Quality**
   - Follows standards
   - Clear and maintainable
   - Proper error handling
   - Performance acceptable

2. **Documentation**
   - Clear instructions
   - Examples provided
   - Updated README
   - Breaking changes noted

3. **Testing**
   - Changes tested
   - Edge cases handled
   - Error scenarios covered

4. **Compatibility**
   - Claude Code compatible
   - No breaking changes
   - Backwards compatible

### Timeline

- **Initial review**: 1-3 days
- **Requested changes**: 2-5 days
- **Approval**: Final review and approval
- **Merge**: After approval

## Reporting Security Issues

**Do not** create public GitHub issues for security vulnerabilities.

Instead, email security details to the maintainer privately. Include:
- Description of vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (optional)

## Questions?

- **Setup issues**: Create GitHub Discussion
- **Feature ideas**: Create GitHub Discussion
- **Bug reports**: Create GitHub Issue
- **General questions**: Ask in Discussions

## Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Thanked in release notes
- Credited in commit history

---

Thank you for contributing to Greet Plugin! 🎉
