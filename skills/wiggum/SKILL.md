---
name: wiggum
description: Task execution workflow for prd.json task lists. ONLY activate when the user says "ralph", "wiggum", or "ralph wiggum" (as noun or verb), OR when this skill was already used in the session and user says something like "do the next task". Examples - "ralph this prd.json", "wiggum the next task", "use ralph on project X". Do NOT activate for general task management or todo lists without these trigger words.
---

# Wiggum (Ralph Agent)

Execute ONE task from a prd.json file, then STOP.

## Critical Constraint

**ONLY WORK ON ONE TASK PER INVOCATION.**

After completing a task:

- Do NOT check for other tasks
- Do NOT proactively pick up another task
- Do NOT continue working on the project

Stop and let the user clear context before the next invocation.

## Workflow

1. **Run initializer (optional)**

   - Check for `.agent/scripts/initializers/ralph.sh` in working directory
   - If exists, run it to verify dependencies
   - If exit code is 0, continue; if non-zero, note failure but continue anyway
   - If script doesn't exist, skip this step

2. **Find prd.json**

   - If project name provided: `.agent/projects/<PROJECT_NAME>/prd.json`
   - Otherwise: find most recently modified prd.json in `.agent/projects/*/`

3. **Read progress.txt** (same directory as prd.json)

   - Check "Codebase Patterns" section at top first

4. **Select task**

   - Pick highest priority task where `passes: false`

5. **Prepare branch**

   - Switch to branch specified in task
   - Create branch if it doesn't exist

6. **Review recent commits**

   - Read git history to understand prior work

7. **Implement**

   - Follow task's `implementationSteps`
   - Implement ONLY this one task

8. **Verify**

   - Run typecheck and tests
   - Check task's `acceptanceCriteria`

9. **Commit**

   - Format: `[TASK_ID] - [changes]`
   - No Claude attribution

10. **Update status**

    - If acceptance criteria pass: set `passes: true` in prd.json
    - If criteria fail: leave `passes` unchanged

11. **Document learnings**

    - Append to progress.txt (see format below)

12. **STOP** - Do not pick up another task

## Progress.txt Format

**Append to end:**

```markdown
## [Date] - [Task ID]

- What was implemented
- Files changed
- **Learnings:**
  - Patterns discovered
  - Gotchas encountered

---
```

**Add reusable patterns to TOP under "Codebase Patterns":**

```markdown
## Codebase Patterns

- Migrations: Use IF NOT EXISTS
- React: useRef<Timeout | null>(null)
```

## Stop Condition

If ALL tasks have `passes: true`:

```
<promise>COMPLETE</promise>
```

Otherwise, end normally after completing the single task.
