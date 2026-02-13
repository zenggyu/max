---
name: gh-issue
description: Manage GitHub Issue discussions for brainstorming and decision-making. Use when Nick initiates a discussion with phrases like "讨论一下"、"我们来聊聊"、"关于 XX 的想法" or any topic proposal that needs tracking in GitHub Issues.
---

# GitHub Issue Discussion Workflow

## Start Discussion

When Nick proposes a new topic:

1. **Gather context**: Read MEMORY.md + memory/YYYY-MM-DD.md (last 2 days) to determine which repository
2. **Confirm repo**: If uncertain which repository, ask Nick to confirm before proceeding
3. **Search issues**: `gh issue list --repo <owner>/<repo> --state open` to find similar open issues
4. **Present options**: List similar issues, ask Nick: reuse existing or create new?
5. **Create if needed**:
   - 与 Nick 确认标题
   - **确定讨论大纲**（至少包括）：
     - 背景
     - 目标
     - 本次议题范围内的要点
     - 不在本次议题范围内的要点
   - 确认后执行：`gh issue create --repo <owner>/<repo> --title "TITLE" --body "<大纲内容>"`

## During Discussion

- When topic drifts, ask: "是否记录本轮讨论到 Issue？"
- On confirmation, execute:
  1. **Add comment** with format:
     - Main points of this round: [distilled core points]
     - Rationale: [record basis for proposals]
     - Changes from prior: [differences from previous conclusions, if any]
     - Reason for change: [explain evolution of thinking]
  2. **Update issue description** with current consensus
  3. **Append to memory/YYYY-MM-DD.md**: Same content as the comment

## Close Discussion

- Final round naturally produces conclusion in description
- Ask Nick to confirm
- `gh issue close <number> --repo <owner>/<repo>`
