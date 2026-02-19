# Agent Skills

A collection of thinking frameworks and decision-making tools as skills for AI coding assistants.

## Installation

### Marketplace (Recommended)

Install directly in Claude Code:

```
/plugin marketplace add neurofoo/agent-skills
```

Or install specific plugin categories:

```
/plugin marketplace add neurofoo/agent-skills/thinking-frameworks
/plugin marketplace add neurofoo/agent-skills/prioritization-skills
/plugin marketplace add neurofoo/agent-skills/creative-skills
/plugin marketplace add neurofoo/agent-skills/learning-skills
/plugin marketplace add neurofoo/agent-skills/retrospective-skills
/plugin marketplace add neurofoo/agent-skills/writing-skills
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/neurofoo/agent-skills.git
cd agent-skills

# Install all skills (user-level, all projects)
mkdir -p ~/.claude/skills
cp -r */ ~/.claude/skills/

# Or project-level (single project)
mkdir -p .claude/skills
cp -r */ .claude/skills/
```

### Install Individual Skills

To install only specific skills:

```bash
# User-level
mkdir -p ~/.claude/skills
cp -r ooda ~/.claude/skills/
cp -r premortem ~/.claude/skills/

# Or project-level
mkdir -p .claude/skills
cp -r ooda .claude/skills/
cp -r premortem .claude/skills/
```

After installation, restart Claude Code. Skills appear in the `/skills` menu and can be invoked directly (e.g., `/ooda`, `/premortem`).

## Available Skills

### Decision & Analysis

| Skill | Invoke | Description |
|-------|--------|-------------|
| **OODA Loop** | `/ooda` | Military-origin decision loop: Observe, Orient, Decide, Act |
| **Pre-Mortem** | `/premortem` | Imagine failure, work backward to prevent it |
| **WRAP** | `/wrap` | Counter decision biases: Widen options, Reality-test, Attain distance, Prepare |
| **Cynefin** | `/cynefin` | Categorize problems as Simple/Complicated/Complex/Chaotic |
| **Red Team** | `/redteam` | Adversarial analysis to find weaknesses and vulnerabilities |
| **SWOT** | `/swot` | Strengths, Weaknesses, Opportunities, Threats analysis |

### Creative & Ideation

| Skill | Invoke | Description |
|-------|--------|-------------|
| **SCAMPER** | `/scamper` | 7 creative prompts: Substitute, Combine, Adapt, Modify, Put to other uses, Eliminate, Reverse |
| **Six Thinking Hats** | `/sixhats` | Parallel thinking from 6 perspectives (facts, feelings, caution, benefits, creativity, process) |
| **Design Thinking** | `/design` | Human-centered design: Empathize, Define, Ideate, Prototype, Test |

### Learning & Understanding

| Skill | Invoke | Description |
|-------|--------|-------------|
| **Feynman Technique** | `/feynman` | Explain simply to find knowledge gaps, then refine |
| **Socratic Method** | `/socratic` | Deep questioning to expose assumptions and reach understanding |
| **Five Whys** | `/5whys` | Iterative root cause analysis |

### Prioritization

| Skill | Invoke | Description |
|-------|--------|-------------|
| **Eisenhower Matrix** | `/eisenhower` | Urgent/Important 4-quadrant prioritization |
| **RICE Scoring** | `/rice` | Quantitative prioritization: Reach x Impact x Confidence / Effort |
| **MoSCoW** | `/moscow` | Must have, Should have, Could have, Won't have |

### Retrospectives & Learning

| Skill | Invoke | Description |
|-------|--------|-------------|
| **After-Action Review** | `/aar` | What was expected? What happened? Why? What next? |
| **Post-Mortem** | `/postmortem` | Blameless incident analysis |
| **Start-Stop-Continue** | `/retro` | Simple 3-column retrospective |

### Strategic

| Skill | Invoke | Description |
|-------|--------|-------------|
| **Wardley Mapping** | `/wardley` | Value chain + evolution strategic mapping |
| **Jobs to Be Done** | `/jtbd` | Understand what job customers hire products for |

### Writing

| Skill | Invoke | Description |
|-------|--------|-------------|
| **Elements of Style** | `/eos-style` | Style guidance based on Strunk & White |
| **Composition** | `/eos-composition` | Principles of composition and structure |
| **Usage** | `/eos-usage` | Words and expressions commonly misused |

## Quick Reference

| When You Need To... | Use |
|---------------------|-----|
| Make a complex decision | `/ooda`, `/wrap` |
| Find root cause | `/5whys`, `/socratic` |
| Brainstorm ideas | `/scamper`, `/sixhats`, `/design` |
| Evaluate risks | `/premortem`, `/redteam` |
| Prioritize work | `/eisenhower`, `/rice`, `/moscow` |
| Learn from failure | `/aar`, `/postmortem` |
| Understand users | `/jtbd`, `/design` |
| Plan strategy | `/wardley`, `/swot`, `/cynefin` |
| Simplify explanation | `/feynman` |
| Challenge assumptions | `/redteam`, `/socratic` |
| Team retrospective | `/retro` |
| Improve writing | `/eos-style`, `/eos-composition`, `/eos-usage` |

## Structure

```
agent-skills/
├── .claude-plugin/
│   └── marketplace.json    # Marketplace manifest
├── [skill-name]/
│   └── SKILL.md            # Frontmatter + instructions + examples
└── ...
```

SKILL.md format:

```yaml
---
name: skillname
description: What this skill does and when to use it.
user-invocable: true
---

# Skill Title

[Instructions for running the analysis]

$ARGUMENTS
```

## Usage Examples

```
/ooda Should I accept this job offer?
/premortem We're launching the new product next week
/5whys The deployment failed
/rice Feature A vs Feature B vs Feature C
/scamper How to improve our checkout flow
/feynman Kubernetes
/swot Our competitive position
/eos-style Review my blog post draft
```

## License

MIT
