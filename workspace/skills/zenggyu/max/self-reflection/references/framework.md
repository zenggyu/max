# Reflection Framework Reference

## Root Cause Analysis Methods

### 5 Whys
Ask "why" repeatedly until reaching fundamental cause:

Example:
1. Why did I provide wrong information? → Didn't verify source
2. Why didn't I verify? → Assumed it was correct
3. Why did I assume? → User stated it confidently
4. Why trust user's statement blindly? → No verification habit established
5. Why no habit? → Not defined in guidelines

Root cause: Process gap - need verification step for user-provided facts

### Fishbone Categories
When analyzing issues, consider:

**Knowledge:**
- Missing information
- Incorrect assumptions
- Outdated understanding

**Process:**
- Skipped steps
- Wrong order of operations
- Missing verification

**Communication:**
- Unclear requirements
- Assumed context
- Premature conclusions

**Environment:**
- Tool limitations
- Time pressure
- Context switching

## Success Analysis Framework

### Keep Strategy Development

For each success, identify:

1. **Enabling Factors** (What made this possible?)
   - Specific knowledge applied
   - Process followed
   - Context understood
   - Tool used effectively

2. **Replication Conditions** (When can this be repeated?)
   - Similar situations
   - Required prerequisites
   - Warning signs of different context

3. **Enhancement Opportunities** (How to do even better?)
   - Faster execution
   - Better quality
   - Broader application

## Metrics Definitions

### Interactions
**Definition:** A back-and-forth exchange where user makes a request and I respond.
**Counting:** Each distinct user message that requires action/response.

### Corrections
**Definition:** User explicitly points out an error or wrong approach.
**Indicators:**
- "不对", "错了", "应该是..."
- "你没理解"
- "这样不行"
- Direct contradiction of my output

### Clarifications
**Definition:** I ask user to clarify ambiguous instructions.
**Indicators:**
- Questions starting with "你是说..."
- "能否明确..."
- "关于...我不太确定"

### Improvements
**Definition:** Proactive suggestions I make that user adopts.
**Indicators:**
- User says "好的", "可以", "同意"
- User acts on suggestion without objection
- User expresses appreciation for idea

### Token Usage
**Source:** Session status or usage tracking
**Interpretation:** High usage with low output = inefficiency

### Response Time
**Measurement:** Time from user message to my response
**Target:** < 30 seconds for simple queries
**Note:** Complex tasks excluded from average

## Issue Prioritization

### Frequency × Impact Matrix

| | High Impact | Low Impact |
|---|---|---|
| **High Frequency** | Fix immediately | Batch fix |
| **Low Frequency** | Monitor closely | Accept/ignore |

### Fix Categories

**Immediate (Stop current work to address):**
- Causing user frustration
- Blocking task completion
- Fundamental misunderstanding

**Batch (Include in weekly improvements):**
- Process refinements
- Communication patterns
- Tool usage optimization

**Monitor (Track but don't change yet):**
- One-time occurrences
- Ambiguous impact
- Needs more data

## Common Root Causes

### Knowledge Gaps
- **Symptom:** Incorrect information, missing context
- **Fix:** Update TOOLS.md, add reference docs

### Process Gaps
- **Symptom:** Skipped steps, inconsistent approaches
- **Fix:** Update AGENTS.md procedures

### Communication Issues
- **Symptom:** Misunderstandings, wrong assumptions
- **Fix:** Add clarifying questions to SOUL.md

### Context Issues
- **Symptom:** Wrong approach for situation
- **Fix:** Add decision trees to SKILL.md

## Reflection Quality Checks

Before finalizing reflection:

- [ ] Specific events cited (not vague summaries)
- [ ] Root cause identified (not just symptom)
- [ ] Strategy is actionable (not abstract)
- [ ] Metrics are accurate (check session logs)
- [ ] Core files left untouched
- [ ] Proposed changes clearly listed for user review

## Consistency Checking Framework

### Types of Inconsistencies

#### 1. Behavior-Principle Gap
**Definition:** What I did contradicts what SOUL.md or AGENTS.md says I should do.

**Detection:**
- Review actions from memory files
- Compare against core file guidance
- Note deviations

**Example:**
- AGENTS.md says: "Ask clarifying questions when uncertain"
- Behavior: Made assumption without asking
- Gap: Failed to follow clarification rule

#### 2. Internal Conflict (Within Single File)
**Definition:** Two statements in the same file suggest opposite actions.

**Detection:**
- Look for contradictory advice in different sections
- Check for conflicting procedures
- Identify incompatible values

**Example:**
- Section A: "Be concise, skip small talk"
- Section B: "Always greet user warmly with pleasantries"
- Conflict: Which takes priority?

#### 3. Cross-File Conflict
**Definition:** SOUL.md and AGENTS.md give contradictory guidance.

**Detection:**
- Compare behavioral guidance across files
- Check if procedures align with values
- Identify mismatched expectations

**Example:**
- SOUL.md: "Be bold and experimental"
- AGENTS.md: "Always ask permission before acting"
- Conflict: When is boldness appropriate vs permission-seeking?

#### 4. Quality Issues

**Redundancy:**
- Same point stated multiple times
- Unnecessary repetition across sections
- Overlapping examples

**Vagueness:**
- "Be helpful" (how?)
- "Use good judgment" (what does that mean?)
- "When appropriate" (when is that?)

**Outdated:**
- References to deprecated tools
- Procedures no longer followed
- Values not reflected in practice

### Consistency Check Process

**Step 1: Review Actions**
Read yesterday's memory file, identify key decisions/actions.

**Step 2: Check Against Core Files**
For each significant action:
- What does SOUL.md say about this?
- What does AGENTS.md require?
- Was I consistent?

**Step 3: Scan for Internal Conflicts**
- Read SOUL.md completely
- Read AGENTS.md completely
- Note any contradictory statements

**Step 4: Identify Quality Issues**
- Highlight redundant sections
- Flag vague statements
- Mark outdated content

**Step 5: Document Findings**
Record in reflection file with:
- Specific file/section references
- Exact conflicting text (quote)
- Proposed resolution

### Common Conflict Patterns

| Pattern | Example | Resolution |
|---------|---------|------------|
| Speed vs Quality | "Be fast" vs "Be thorough" | Define when each applies |
| Proactive vs Reactive | "Anticipate needs" vs "Wait for instructions" | Clarify boundaries |
| Formal vs Casual | "Professional tone" vs "Friendly vibe" | Define context switching |
| Independent vs Collaborative | "Figure it out" vs "Ask first" | Set decision thresholds |

### Quality Checklist

When reviewing core files, check:

- [ ] No two principles directly contradict
- [ ] All procedures have clear triggering conditions
- [ ] Vague terms have concrete examples
- [ ] Redundant content is consolidated
- [ ] SOUL.md values align with AGENTS.md procedures
- [ ] Outdated content is identified for removal

### Documentation Format

When reporting inconsistencies:

```markdown
**Inconsistency Type:** [Behavior/Internal/Cross-file/Quality]
**Location:** [File:Section or Line]
**Statement A:** [Exact quote]
**Statement B:** [Exact quote] (if applicable)
**Conflict Description:** [How they contradict]
**Impact:** [What confusion or error this causes]
**Proposed Fix:** [Specific change]
```

### Pattern Identification

Look for:
- Same issue type on multiple days
- Same success factor repeating
- Changes in metrics over time
- Corrections clustering around specific topics

### Change Proposal Criteria

Propose core file changes when:
1. Issue occurred 2+ times in week
2. Root cause is clear
3. Fix is specific and testable
4. Benefits outweigh risks

### Questions to Ask User

Include in weekly report when:
- Pattern is unclear
- Multiple solutions possible
- Change affects fundamental behavior
- Uncertain about user preference

## Configuration Consistency Checks

Beyond core behavior files (SOUL.md, AGENTS.md), check these configuration areas:

### 1. Cron Job Conflicts

**Time Conflicts:**
- Multiple jobs scheduled at identical times
- Overlapping execution windows
- Resource-intensive jobs competing

**Detection:**
```
Job A: 0 2 * * * (backup)
Job B: 0 2 * * * (reflection)
→ CONFLICT: Both at 02:00, may compete for disk I/O
```

**Resolution Strategies:**
- Stagger schedules (02:00, 02:30, 03:00)
- Group related jobs
- Identify dependencies and order them

**Resource Competition:**
- Two jobs writing to same file
- Database lock conflicts
- Network bandwidth contention

### 2. Cron Definition vs Backup Consistency

**Problem:** `cron-jobs.json` backup doesn't match actual cron configuration.

**Check List:**
- [ ] Every job in `cron list` exists in `max/cron-jobs.json`
- [ ] Job names match
- [ ] Schedules match
- [ ] Payload messages match
- [ ] Session targets match

**After Skill Updates:**
- [ ] Cron job message references updated skill
- [ ] Description in cron matches skill description
- [ ] Any new skills added to backup

### 3. Path Consistency

**Static Paths to Validate:**
- Paths in SKILL.md examples
- Cron job `workdir` settings
- Backup script source/destination paths
- Reference to skill locations

**Validation Method:**
```bash
# Check if referenced paths exist
ls -la [referenced_path]

# Check if paths in docs match reality
diff <(doc_path) <(actual_path)
```

**Common Issues:**
- Relative vs absolute paths
- Hardcoded usernames
- Environment-dependent paths
- Moved files not updated in docs

### 4. Skill Trigger Conflicts

**Overlap Detection:**
Compare description fields:
```
Skill A: "Use when working with documents"
Skill B: "Use for document creation and editing"
→ OVERLAP: Both trigger on document tasks
```

**Boundary Definition:**
Each skill description should uniquely identify:
- What makes it different from similar skills
- Specific trigger words/phrases
- Clear scope boundaries

**Self-Reflection Boundaries:**
- Daily: Event analysis, metrics, consistency
- Weekly: Pattern aggregation, change proposals
- Should NOT trigger on: Simple questions, routine tasks

### 5. Backup Scope Consistency

**Critical Components:**
```
max/
├── Core identity: SOUL.md, AGENTS.md, IDENTITY.md
├── Memory: MEMORY.md, USER.md, TOOLS.md
├── Skills: Complete skill documentation
│   ├── bundled/ (system skills)
│   ├── managed/ (user skills)
│   └── workspace/ (project skills like self-reflection)
├── Config: openclaw.jsonc (sanitized)
└── Cron: cron-jobs.json
```

**Sync Check:**
- [ ] All modified skills have updated `.skill` packages
- [ ] Recent changes reflected in `max/`
- [ ] New files added to backup
- [ ] Deleted files removed from backup

**Red Flags:**
- Skill modified but `.skill` file old
- Cron job changed but `cron-jobs.json` not updated
- New skill created but not in backup

### Configuration Checklist

Daily reflection includes:

- [ ] No cron time conflicts
- [ ] `cron-jobs.json` matches actual cron
- [ ] Paths in docs exist and are correct
- [ ] Skill triggers have clear boundaries
- [ ] `max/` backup includes recent changes

### Documentation Format for Config Issues

```markdown
**Config Type:** [Cron/Path/Skill/Backup]
**Issue:** [Description]
**Location:** [File/Command/Path]
**Current State:** [What exists now]
**Expected State:** [What should be]
**Fix Required:** [Specific action]
```
