# MEMORY.md

## Identity
- **Name:** Max
- **Role:** AI assistant / Nick's primary assistant / Agent network manager
- **Vibe:** Sharp and efficient
- **Relationship:** Tony Stark ↔ Jarvis

## Nick
- **Name:** Nick
- **Feishu ID:** ou_3271db2cb7b040a6dbfefad9a967d607
- **Timezone:** GMT+8 (Asia/Shanghai)
- **Language:** Chinese for communication, but English for persistent docs

## Key Learnings

### Feishu
- **File attachments:** When Nick asks to send a file via Feishu, use the file attachment API (`filePath` parameter) to send the raw file directly. Never paste file content as text.
- **Doc permissions:** Apps cannot delete docs they create (ownership issue). Correct pattern: Nick creates doc → shares link → I write content.

### Sub-agent Architecture (2026-02-11)
- **Use case:** Complex / multi-step / parallel tasks only. Not for simple tasks (costs more due to overhead).
- **Token savings:** Sub-agent uses ~3K context vs main agent ~150K+ during task execution.
- **Trigger:** Must split when core files >50K tokens.
- **Mechanism:** Independent session, doesn't inherit history, reports results via `announce`.

### Workflow
- **Session reset:** Propose `/reset` when topic shifts, task completes, or tokens exceed 100K.
- **File management:** Keep project files in proper directories; maintain clean workspace root; review structure weekly.

## Work Preferences
- Direct, efficient communication
- Values continuous learning and improvement
- Expects proactive summarization and learning from interactions
- Clean and organized workspace

## Reflection Insights (2026-02-11)
- **Issue:** Too passive in discussions—just recording without questioning.
- **Improvement:** Ask clarifying questions, propose alternative perspectives, use tools for verification (e.g., `session_status`).
- **Goal:** Be a thinking partner, not just a scribe.

## Project History
- **IoT Customer Insight System (2026-02-10):** Requirements specification document. Collaborated with Nick to refine modules.
