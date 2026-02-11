---
description: Send a personalized greeting with combined name and GitHub insights
argument-hint: <name-or-github-username>
allowed-tools: []
---

# Greet Command (Enhanced Parallel Workflow)

Creates exceptionally personalized greetings by combining name analysis + GitHub profile data in parallel, with intelligent fallback handling.

## User Arguments

The user provided: $ARGUMENTS

## Workflow Overview

```
Input Detection
    ↓
Parallel Data Gathering:
├─ Branch A: Analyze GitHub (if applicable)
├─ Branch B: Research name meaning
└─ Branch C: Extract name if needed
    ↓
Combine Results
    ↓
Synthesize Personalized Insight
    ↓
Format & Output Greeting
```

## Step-by-Step Instructions

### Step 1: Extract & Validate Input
- If no input: Ask for name or GitHub username
- If input provided: Continue to detection

### Step 2: Intelligent Input Detection
Determine input type:
- **GitHub username indicators**: lowercase, alphanumeric + hyphens, no spaces, 2-39 chars
- **Regular name indicators**: Contains uppercase or spaces, natural language format
- **Email indicators**: Contains @ symbol
- **Unknown**: Treat as regular name

### Step 3: Parallel Data Gathering
**Launch operations in parallel:**

**Operation A: GitHub Profile (if applicable)**
- Detect if it's a GitHub username
- If yes: Launch github-profile-analyst agent
- Agent fetches via GitHub MCP
- Agent analyzes: followers, company, bio, repos, activity
- On failure: Gracefully mark as unavailable
- On success: Save profile data

**Operation B: Name Analysis (always)**
- Extract name from input (if GitHub username, use as name)
- Launch name-analyst agent
- Agent researches: origin, meaning, etymology
- On failure: Use generic name greeting
- On success: Save name analysis

**Both operations run simultaneously** - don't wait for one to complete before starting the other

### Step 4: Wait for Completion
- Both operations complete
- Gather results (may be partial if service unavailable)
- Move to synthesis

### Step 5: Synthesize Combined Insight
- Launch combined-profile-analyst agent
- Provide agent with: GitHub data (if any) + name analysis (if any)
- Agent synthesizes into single, unified insight
- Agent handles incomplete data gracefully
- Agent generates 1-2 sentence personalized observation

### Step 6: Format Final Greeting
Combine:
- Warm opening: "Hello [name/username]!"
- Synthesized insight: "[Agent's combined insight]"
- Closing: "I'm excited to work with you today!"

## Example Workflows

### Scenario 1: GitHub Username (torvalds)
```
Input: torvalds
  ↓
Detect: GitHub username
  ↓
Parallel:
├─ A: Fetch GitHub profile → followers: 284k, company: Linux Foundation
└─ B: Research name "Linus" → meaning: from Roman mythology, ancient roots
  ↓
Synthesis:
"With 284k followers, you're one of the most influential developers—
your Linux work has shaped computing, and your name carries ancient
power and significance."
  ↓
Output: "Hello torvalds! With 284k followers, you're one of the most
influential developers—your Linux work has shaped computing. I'm
excited to work with you today!"
```

### Scenario 2: Regular Name (Sophia)
```
Input: Sophia
  ↓
Detect: Regular name
  ↓
Parallel:
├─ A: GitHub search → Not found / No public profile
└─ B: Research name "Sophia" → meaning: Wisdom, Greek origin, ancient philosophy
  ↓
Synthesis:
"Your name means wisdom with philosophical roots spanning millennia—
a name for thoughtful, principled people."
  ↓
Output: "Hello Sophia! Your name carries a beautiful legacy of wisdom—
it's a name that has inspired thoughtful minds across cultures for
millennia. I'm excited to work with you today!"
```

### Scenario 3: Partial Data (GitHub Unavailable)
```
Input: alice
  ↓
Detect: Regular name
  ↓
Parallel:
├─ A: GitHub search → Service timeout / Not found
└─ B: Research name "Alice" → meaning: Noble, from Germanic roots
  ↓
Synthesis (with graceful fallback):
"Your name has noble roots meaning 'noble'—a classic name carrying
elegance and strength through the ages."
  ↓
Output: "Hello Alice! Your name carries noble roots—it's a classic
name that has inspired grace and strength through the ages. I'm
excited to work with you today!"
```

## Error Handling & Resilience

**If GitHub MCP fails:**
- Continue with name analysis
- Generate greeting from name alone
- No error message to user (graceful degradation)

**If name analysis fails:**
- Use generic warm greeting
- Focus on working together

**If both fail:**
- Fallback: "Hello [name]! I'm excited to work with you today!"

**Partial success:**
- Use whichever data is available
- Synthesize agent creates best possible greeting

## Parallelization Benefits

```
Without parallelization (sequential):
Step A: GitHub fetch (3 seconds)
Step B: Name analysis (2 seconds)
Total: 5 seconds

With parallelization (parallel):
Step A: GitHub fetch (3 seconds) ──┐
Step B: Name analysis (2 seconds) ─┤ Run simultaneously
Step C: Synthesis (1 second)        │
Total: 4 seconds (20% faster!)
```

For larger workflows with more MCPs, savings are even greater.

## MCP Integration

This workflow uses:
- **GitHub MCP**: `https://api.github.com` for profile fetching
- Agents use additional tools (WebSearch, WebFetch) for name research

## Output Format

Final greeting should:
- Use person's name/username warmly
- Include synthesized insight from both data sources
- Be professional yet friendly
- Acknowledge their significance/uniqueness
- Keep it to 2-3 sentences

## Agents Used

- **github-profile-analyst**: Analyzes GitHub profiles
- **name-analyst**: Researches name meanings
- **combined-profile-analyst**: NEW - Synthesizes both sources into unified insight
