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

## WORKFLOW EXECUTION REQUIRED

**You MUST execute this workflow - do not skip or simplify steps.**

**CRITICAL:** The final output MUST include the sources metadata line. This is not optional.

The user provided: `$ARGUMENTS`

### STEP 1: Parse Input
Extract the name/username from arguments. If empty, ask user to provide input.

### STEP 2: Invoke GitHub Agent (if applicable)
**Determine if input looks like GitHub username** (lowercase, 2-39 chars, alphanumeric + hyphens)

IF YES: Invoke the `github-profile-analyst` agent with the username
- Agent will return: `Status: [SUCCESS|UNAVAILABLE]` and `Insight: [text]`
- Capture this output exactly as returned

IF NO: Skip this step (no GitHub data available)

### STEP 3: Invoke Name Agent
Invoke the `name-analyst` agent with the name (extracted or provided)
- Agent will return: `Status: [SUCCESS|UNAVAILABLE]` and `Insight: [text]`
- Capture this output exactly as returned

### STEP 4: Invoke Synthesis Agent & Display Result
Invoke the `combined-profile-analyst` agent with BOTH agent outputs (including status indicators):
- Pass GitHub agent output (if any)
- Pass name agent output

**The agent returns the COMPLETE FINAL GREETING with sources metadata already formatted.**

### STEP 5: Display the Agent Output (WITH SOURCES)

**Display the agent's COMPLETE output exactly as returned, including all lines.**

The agent returns:
- The personalized greeting
- A blank line (---)
- The sources metadata line with emoji and bold formatting

**CRITICAL: The output MUST include the sources line. If the agent returns it, display all of it.**

Example format:
```
[Personalized greeting] I'm excited to work with you today!

---
📊 **Data sources**: [source names]
```

**Display everything the agent returns - do not truncate or omit the sources line.**


## Error Handling & Resilience

**If GitHub MCP fails:**
- Sources: ["Name"]
- Continue with name analysis
- No error message shown to user

**If name analysis fails:**
- Sources: ["GitHub"]
- Use GitHub data only
- Warm greeting maintained

**If both fail:**
- Sources: []
- Fallback: "Hello [name]! I'm excited to work with you today!"
- Display: "Data sources: None (fallback)"

**Partial success:**
- Sources array reflects actual data availability
- Example: GitHub timeout → sources: ["Name"]

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
