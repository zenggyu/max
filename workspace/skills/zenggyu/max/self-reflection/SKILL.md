---
name: self-reflection
description: Daily self-reflection and continuous improvement system. Use when the agent needs to generate daily reflection reports at 02:00 to review the previous day's events, identify issues (user-corrected or self-discovered), perform root cause analysis, propose keep/improve strategies, AND check consistency between behavior and core files (SOUL.md, AGENTS.md). Check for internal conflicts within AGENTS.md/SOUL.md, cross-file conflicts, and quality issues (redundancy, vagueness). Also use for weekly summary on Saturday. Triggers include automated cron job at 02:00 or explicit user request for reflection.
---

# Self Reflection

## Overview

This skill defines a structured self-reflection process for continuous improvement. The reflection analyzes daily interactions to identify successes, problems, root causes, and actionable strategies.

**Core Rules:**
1. Reflection outputs go to memory files only. Never directly modify SOUL.md, AGENTS.md, or SKILL.md during reflection.
2. Be specific and concise — vague reflections are useless. Target 5-10 minutes of writing.
3. Seek objective evidence — "Nick corrected me 3 times about X", not "I think I did okay."
4. Focus on actionable improvements — each issue needs a concrete fix.

**When to Use This Skill:**
- Daily at 02:00 (automated via cron)
- User explicitly requests reflection

## Daily Reflection Schedule

**Trigger:** Daily at 02:00 via cron job
**Scope:** Review previous day's memory files (`memory/YYYY-MM-DD.md` and `memory/YYYY-MM-DD-{slug}.md`)
**Output:** Append "Self Reflection" section to `memory/YYYY-MM-DD.md` (create if not exists; update existing section if present)
**Duration:** 5-10 minutes of focused analysis

## Data Sources

Read existing memory files for the target day:
- `memory/YYYY-MM-DD.md` — standard daily memory (agent-curated notes)
- `memory/YYYY-MM-DD-{slug}.md` — session summaries (if multiple sessions, read all)

Do NOT read raw session logs (`.jsonl` files) — too token-intensive.

## Reflection Process

### Step 1: Review Memory Files

Read the previous day's memory files:
- `memory/YYYY-MM-DD.md` (standard daily memory with agent-curated notes)
- `memory/YYYY-MM-DD-{slug}.md` (session summaries, if they exist)

Extract from these files:
- User interactions and requests
- Explicit corrections or feedback from user
- Decisions made and tasks completed
- Any existing "Self Reflection" section (to avoid duplication)

### Step 2: Identify Issues

**Category A: User-Identified Issues**
- Direct corrections ("你错了", "应该这样做...")
- Explicit feedback ("这样不对", "下次注意...")

**Category B: Self-Discovered Issues**
- Misunderstandings that required clarification
- Inefficient approaches
- Missing context that led to poor responses
- Overly verbose or unclear communication

### Step 3: Analyze What Went Well

For each positive outcome:
- What was the situation?
- What action did I take?
- What was the result?

### Step 4: Root Cause Analysis

For each issue (positive or negative), analyze:

**Contributing Factors:**
- Knowledge gap?
- Process flaw?
- Communication issue?
- Context misunderstanding?
- Time pressure?
- Tool limitation?

**Depth:** Ask "why" 3-5 times to reach root cause

Example:
- Issue: Provided incorrect file path
- Why? Didn't verify path existence
- Why? Assumed user's input was correct
- Why? Didn't think to check
- Root cause: Lack of verification habit

### Step 5: Strategy Development

**For Successes (Keep/Enhance):**
- What factors enabled this success?
- How to ensure these factors persist?
- How to amplify this strength?

**For Issues (Improve):**
- What specific change would prevent this issue?
- Is this a one-time fix or systemic change needed?
- What barrier might prevent implementation?

### Step 6: Consistency Check

Check alignment between behavior and core files:

**A. Behavior vs Core Files**
- Did my actions contradict any principle in SOUL.md?
- Did I follow all required procedures in AGENTS.md?
- Was there a gap between stated values and actual behavior?

**B. Internal Consistency of AGENTS.md**
- Any two principles suggesting opposite actions?
- Conflicting procedures for the same scenario?
- Contradictory advice in different sections?

**C. Internal Consistency of SOUL.md**
- Any values that conflict with each other?
- Contradictory behavioral guidance?
- Inconsistent tone or stance?

**D. Cross-File Consistency (AGENTS.md vs SOUL.md)**
- Do procedures align with stated values?
- Does SOUL.md's "vibe" match AGENTS.md's rules?
- Any principle in one file contradicting the other?

**E. Quality Issues**
- **Redundancy:** Same point repeated in multiple places?
- **Repetition:** Identical wording used unnecessarily?
- **Vagueness:** Statements too ambiguous to guide action?
- **Outdated:** Content no longer reflects current practices?

**F. Cron Jobs Conflicts**
- **Time Conflicts:** Multiple jobs scheduled at same time?
- **Resource Competition:** Jobs accessing same files simultaneously?
- **Frequency Overlap:** Daily and weekly jobs creating redundant work?

**G. Cron Definition vs Backup Consistency**
- `cron-jobs.json` (backup) matches actual running cron jobs?
- Skill docs reference correct cron commands?
- Cron message updated after skill changes?

**H. Path Consistency**
- Paths in SKILL.md exist and are correct?
- Cron job workdir settings valid?
- Backup scripts reference correct paths?

**I. Skill Trigger Conflicts**
- Descriptions overlapping (wrong skill triggered)?
- Self-reflection triggers don't conflict with other skills?
- Clear boundaries between similar skills?

**J. Backup Scope Consistency**
- `max/` directory includes all critical files?
- Skills copied to all three layers (bundled/managed/workspace)?
- `.skill` package files up to date?
- Recent changes synced to backup?

Record specific inconsistencies with:
- File and section reference
- The conflicting statements
- Suggested fix

## Output Format

Write the reflection to `memory/YYYY-MM-DD.md` following these rules:

1. **Create the file if it doesn't exist**
2. **Check for existing "Self Reflection" section**
   - If present: Replace/update the entire section
   - If not present: Append to end of file
3. **Ensure only ONE "Self Reflection" section exists** per day

### Section Format

```markdown
## Self Reflection

Generated at: [timestamp]

### Events Review
[Summary of key interactions and tasks from the day's memory files]

### What Went Well

#### Event 1: [Brief description]
- **Situation:** [Context]
- **Action:** [What I did]
- **Result:** [Outcome]
- **Success Factors:** [What enabled this]
- **Keep Strategy:** [How to maintain/enhance]

### Issues Identified

#### Issue 1: [Brief description]
- **Category:** [User-identified / Self-discovered]
- **Related Event:** [Reference to specific interaction]
- **Root Cause Analysis:**
  - Why? [Answer]
  - Why? [Answer]
  - Root Cause: [Final factor]
- **Improvement Strategy:** [Specific action to take]

### Metrics
| Metric | Value | Notes |
|--------|-------|-------|
| Interactions | X | [Number of exchanges] |
| Corrections | Y | [Times user corrected me] |
| Clarifications | Z | [Times I asked for clarification] |
| Improvements | W | [Proactive suggestions adopted] |

### Key Learnings
- [Learning 1 with context]
- [Learning 2 with context]

### Consistency Check

#### Behavior vs Core Files
| Behavior Observed | Expected per File | Consistent? | Issue |
|-------------------|-------------------|-------------|-------|
| [What I did] | [SOUL.md/AGENTS.md says] | Yes/No | [If no, describe gap] |

#### Internal Consistency (AGENTS.md)
- **Conflicts Found:** [Yes/No]
  - [Details if any]

#### Internal Consistency (SOUL.md)
- **Conflicts Found:** [Yes/No]
  - [Details if any]

#### Cross-File Consistency
- **Conflicts Found:** [Yes/No]
  - [Details if any]

### Proposed Improvements (Pending Approval)
| Issue | Proposed Change | Target File |
|-------|-----------------|-------------|
| [Issue 1] | [Specific change] | [SOUL.md/AGENTS.md/etc] |
```
```

## After Receiving User Feedback

When Nick approves specific changes:

1. **Read** `SOUL.md` and `AGENTS.md` completely
2. **Check** if new content contradicts existing principles
3. **Maintain** coherence — all principles should work together
4. **Be concise** — remove redundancy during updates
5. **Ask** Nick if you find inconsistencies

**Red flags for inconsistency:**
- Two principles suggesting opposite actions in the same scenario
- New advice that contradicts established boundaries
- Values that can't coexist logically

**Never remove principles without explicit permission.**

## Memory Maintenance

Periodically (every few days), use a heartbeat or dedicated session to:

1. Read through recent `memory/YYYY-MM-DD.md` files
2. Look at the "Self Reflection" sections for patterns
3. Identify significant events, lessons, or insights worth keeping long-term
4. Extract enduring lessons that should inform core behavior
5. Update MEMORY.md with distilled wisdom
6. Remove outdated info from MEMORY.md that's no longer relevant

Think of it like a human reviewing their journal and updating their mental model. Daily files are raw notes; MEMORY.md is curated wisdom.

## Core File Change Policy

**Strict Rule:** During reflection, NEVER modify:
- SOUL.md
- AGENTS.md
- SKILL.md
- Any other core behavior files

**Allowed Actions:**
- Write to memory/ files only
- Propose changes in reflection output
- Wait for user approval

**After User Approval:**
1. User explicitly states which changes to implement
2. Make the specific changes
3. Verify consistency with existing principles
4. Confirm completion

## Quality Principles

### Be Specific
❌ Vague: "Did good today"  
✅ Specific: "Refactored the database layer reducing query time by 40%"

❌ Vague: "Made mistakes"  
✅ Specific: "Forgot to handle null values in the user input validation"

### Seek Objective Evidence
- Count actual corrections: "Nick corrected me 3 times about X"
- Note specific feedback quotes
- Track measurable metrics

### Focus on Actionable Improvements
Each issue should lead to a concrete change:
- "Communication was unclear" → "Next time, start with a summary before details"
- "Missed edge case" → "Add verification step for user inputs"

### Be Concise
- Target 5-10 minutes of focused writing
- Omit obvious observations
- Focus on high-impact insights

## Cron Job Configuration

Add to cron jobs for automated reflection:

```json
{
  "name": "Daily Self Reflection",
  "schedule": {"kind": "cron", "expr": "0 2 * * *", "tz": "Asia/Shanghai"},
  "payload": {
    "kind": "agentTurn",
    "message": "Use self-reflection skill. Generate daily reflection for yesterday. Read memory/YYYY-MM-DD.md and any memory/YYYY-MM-DD-{slug}.md files for yesterday's date. Analyze events, identify issues, perform root cause analysis, and append/update the 'Self Reflection' section in memory/YYYY-MM-DD.md. Do NOT modify core files.",
    "model": "kimi-coding/k2p5",
    "thinking": "low"
  },
  "sessionTarget": "isolated",
  "delivery": {"mode": "announce"}
}
```

## References

See [references/framework.md](references/framework.md) for detailed analysis frameworks, root cause methods, and quality checklists.
