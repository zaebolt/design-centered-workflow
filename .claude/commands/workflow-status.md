# Workflow Status Command

**Command:** /workflow-status
**Description:** View, manage, and clean up all workflow states
**Category:** Workflow

## Purpose

This command helps users:
- See all saved workflows (in-progress and completed)
- Clean up old/completed workflows
- Delete specific workflows
- Archive completed workflows
- Get disk space back

## Execution Steps

### Step 1: Scan for All Workflow States

```
1. List all files in .claude/state/
2. Filter for workflow-*.json files
3. Read each file and extract:
   - timestamp
   - problem_statement
   - current_phase
   - workflow_status (infer "in-progress" if missing and current_phase != "complete")
   - checkpoints_completed
   - last_updated (if present, else use timestamp)
   - File size
4. Sort by last_updated or timestamp (newest first)
5. Group by status: in-progress, complete
```

### Step 2: Display Workflow Status

```markdown
📊 Workflow Status Overview

**Total Workflows:** {total_count}
**In Progress:** {in_progress_count}
**Completed:** {completed_count}
**Total Storage:** {total_size_mb} MB

---

## In-Progress Workflows

{If none:}
No workflows in progress.

{For each in-progress workflow:}

### {N}. {problem_statement}
- **Started:** {timestamp} ({relative_time})
- **Last Updated:** {last_updated} ({relative_time})
- **Phase:** {current_phase}
- **Progress:** {checkpoints_completed.length}/4 checkpoints
- **File:** workflow-{timestamp}.json ({file_size_kb} KB)
- **Next:** {next_action}

---

## Completed Workflows

{If none:}
No completed workflows.

{For each completed workflow:}

### {N}. {problem_statement}
- **Completed:** {timestamp} ({relative_time})
- **Output:** {design.prototype_path}
- **File:** workflow-{timestamp}.json ({file_size_kb} KB)

---

## Actions

**Resume a workflow:**
- `/design-flow` and select workflow by number

**Clean up:**
- **clean completed** - Delete all completed workflow states
- **clean old** - Delete workflows older than 30 days
- **delete [N]** - Delete specific workflow N
- **archive** - Move completed workflows to .claude/state/archive/

Your choice (or 'cancel' to exit):
```

### Step 3: Handle User Actions

#### Action: clean completed

```
1. Count completed workflows
2. Confirm with user:

⚠️  About to delete {count} completed workflow state files.

This will free up approximately {size_mb} MB.

Note: Generated prototypes in output/ will NOT be deleted.

Are you sure? (yes/no):

3. If yes:
   - Delete each completed workflow file
   - Show results:

✅ Deleted {count} completed workflow states
💾 Freed {size_mb} MB
```

#### Action: clean old

```
1. Find workflows with timestamp > 30 days old
2. Count them
3. Confirm with user:

⚠️  About to delete {count} workflows older than 30 days.

Status breakdown:
- In-progress: {in_progress_count}
- Completed: {completed_count}

This will free up approximately {size_mb} MB.

Are you sure? (yes/no):

4. If yes:
   - Delete each old workflow file
   - Show results:

✅ Deleted {count} old workflow states
💾 Freed {size_mb} MB
```

#### Action: delete [N]

```
1. Validate N is a valid workflow number
2. Load that workflow's details
3. Confirm with user:

⚠️  About to delete workflow {N}:

Problem: "{problem_statement}"
Status: {workflow_status}
Phase: {current_phase}
{If prototype exists: "⚠️  Prototype exists at: {prototype_path}"}

This will delete the state file but keep any generated files.

Are you sure? (yes/no):

4. If yes:
   - Delete the state file
   - Show result:

✅ Deleted workflow state
{If prototype exists: "ℹ️  Prototype still available at: {prototype_path}"}
```

#### Action: archive

```
1. Count completed workflows
2. Create archive directory if needed:
   mkdir -p .claude/state/archive/

3. Confirm with user:

📦 About to archive {count} completed workflows.

They will be moved to: .claude/state/archive/

This keeps them for reference but removes them from active list.

Continue? (yes/no):

4. If yes:
   - Move each completed workflow to archive/
   - Show results:

✅ Archived {count} workflows
📁 Location: .claude/state/archive/

To view archived workflows, run: /workflow-status --archived
```


## Options and Flags

### --archived

Show archived workflows:

```
/workflow-status --archived
```

Output:
```markdown
📦 Archived Workflows

{For each archived workflow:}
### {N}. {problem_statement}
- **Completed:** {timestamp}
- **Output:** {prototype_path}
- **File:** archive/workflow-{timestamp}.json

**Restore:**
- **restore [N]** - Move back to active states

**Delete:**
- **delete [N]** - Permanently delete
```

### --all

Show everything (in-progress, completed, archived):

```
/workflow-status --all
```


## Integration with /design-flow

The two commands work together:

```
/design-flow          → Start/resume workflows (shows only in-progress)
/workflow-status      → Manage all workflows (in-progress + completed)
/workflow-status --archived  → View archived workflows
```

## State File Size Management

If state files grow too large (>1MB each), show warning:

```
⚠️  Warning: Some workflow state files are large.

Large files:
- workflow-2026-01-15.json: 1.2 MB
- workflow-2026-01-16.json: 2.1 MB

This may happen if concepts or PRDs contain large amounts of text.

Consider:
1. **archive** - Archive completed workflows
2. **clean old** - Remove workflows older than 30 days
3. **delete [N]** - Delete specific large workflows
```

## Automatic Cleanup Suggestions

After showing status, if certain conditions are met, suggest cleanup:

```
💡 Suggestion: You have {N} completed workflows from over 30 days ago.
   Run "clean old" to free up {size_mb} MB.
```

or

```
💡 Suggestion: You have {N} completed workflows.
   Run "archive" to move them out of the active list.
```

## Success Criteria

✅ Users can see all workflows at a glance
✅ Users can clean up completed workflows safely
✅ Users can delete specific workflows
✅ Users can archive workflows for reference
✅ Storage space is managed efficiently
✅ Generated files (prototypes) are never deleted accidentally
✅ Clear warnings before any destructive action
