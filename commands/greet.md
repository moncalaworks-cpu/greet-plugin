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

### STEP 4: Invoke Synthesis Agent
Invoke the `combined-profile-analyst` agent with BOTH agent outputs (including status indicators):
- Pass GitHub agent output (if any)
- Pass name agent output
- Agent will parse statuses and return BOTH:
  - `Greeting: [Your synthesized insight]`
  - `Sources: [JSON array of sources]`
- Capture the ENTIRE response including both Greeting and Sources

### STEP 5: CRITICAL - Extract Sources and Format Final Output

**MUST DO:**
1. Parse the synthesis agent response line by line
2. Find the line starting with `Sources: ` - this contains the JSON array
3. Extract the array value (e.g., `["GitHub", "Name"]` or `["Name"]` or `[]`)
4. Convert array to comma-separated string for display
5. Format the final greeting with sources metadata

**Exact format:**
```
Hello [name/username]! [Greeting text from synthesis agent] I'm excited to work with you today!

---
📊 **Data sources**: [comma-separated sources]
```

**Examples:**
- If sources array is `["GitHub", "Name"]` → display: `GitHub, Name`
- If sources array is `["Name"]` → display: `Name`
- If sources array is `[]` → display: `None (fallback)`

### Step 6: Format Final Greeting with Source Metadata

Parse the synthesis agent's output to extract:
- Greeting text (from **Greeting**: line)
- Sources array (from **Sources**: line)

Output format:
```
Hello [name/username]! [Synthesized insight] I'm excited to work with you today!

---
📊 **Data sources**: [comma-separated source names, or "None (fallback)" if empty]
```

Examples:

**Both sources:**
```
Hello torvalds! With 284,000 followers, you're one of the most influential developers—your Linux work has shaped computing. I'm excited to work with you today!

---
📊 **Data sources**: GitHub, Name
```

**Single source:**
```
Hello Sophia! Your name carries a beautiful legacy of wisdom across cultures for millennia. I'm excited to work with you today!

---
📊 **Data sources**: Name
```

**Fallback (no sources):**
```
Hello Alice! I'm excited to work with you today!

---
📊 **Data sources**: None (fallback)
```

## Examples

**GitHub Username (torvalds):**
```
Hello torvalds! With 284,000+ followers, you're one of the most
influential developers—your Linux work has shaped computing. I'm
excited to work with you today!

---
📊 **Data sources**: GitHub, Name
```

**Regular Name (Sophia):**
```
Hello Sophia! Your name carries a beautiful legacy of wisdom across
cultures for millennia. I'm excited to work with you today!

---
📊 **Data sources**: Name
```

**Graceful Fallback (both unavailable):**
```
Hello Alice! I'm excited to work with you today!

---
📊 **Data sources**: None (fallback)
```

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
