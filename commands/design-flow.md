---
user-invocable: true
description: Start a design-centered product development workflow with user research, concept exploration, and prototype generation
argument-hint: [product idea]
---

# Design-Centered Workflow

## Product Idea

**Problem Statement:** $ARGUMENTS

If no arguments were provided, ask the user: "What product or feature would you like to build?" and wait for their response. Once you have the product idea (either from $ARGUMENTS or from the user's response), proceed to the tasks below.

## Your Task

Follow the complete design-centered workflow to help the user build their product idea.

### Step 0: Check for Existing Workflows (Silent Background Check)

**Before doing anything else, silently check for existing in-progress workflows:**

1. List all files in `.claude/state/` matching `workflow-*.json`
2. Filter for workflows where `current_phase != "complete"` OR `workflow_status != "complete"`
3. Sort by `last_updated` or `timestamp` (newest first)
4. Count in-progress workflows

**If 0 workflows found:** Proceed to Step 1 (Initialize New Workflow)

**If 1 workflow found AND no $ARGUMENTS provided:**
Present simple resume option:
```
👋 Welcome back!

I found your in-progress workflow: "{problem_statement}"
Currently in {current_phase} phase ({checkpoints_completed_count}/4 checkpoints completed)

Would you like to:
• **continue** - Resume where you left off
• **new** - Start a different product idea

Your choice:
```
Then handle response: `continue` → Step 2 (Resume), `new` → Step 1 (Initialize)

**If 1 workflow found AND $ARGUMENTS provided:**
User wants something new. Proceed to Step 1 (Initialize New Workflow)

**If 2+ workflows found:**
Present workflow selection:
```
👋 You have {N} workflows in progress:

{For each workflow:}
{N}. "{problem_statement}" - {current_phase} phase

Which would you like to:
• **continue [N]** - Resume workflow N (e.g., "continue 1")
• **new** - Start a fresh workflow{if $ARGUMENTS: " for: {$ARGUMENTS}"}

Your choice:
```
Then handle response: `continue [N]` → Step 2 (Resume workflow N), `new` → Step 1 (Initialize)

---

### Step 1: Initialize New Workflow

Create a new state file at `.claude/state/workflow-{timestamp}.json` with the following structure:

```json
{
  "timestamp": "{ISO-8601-timestamp}",
  "problem_statement": "{user's product idea from $ARGUMENTS or their response}",
  "current_phase": "discovery",
  "phase_data": {
    "discovery": { "iteration": 1, "user_approved": false, "competitive_skipped": false },
    "exploration": { "iteration": 1, "concepts": [], "selected_ids": [] },
    "design": { "status": "pending" },
    "validation": { "status": "pending" }
  },
  "checkpoints_completed": []
}
```

---

### Step 2: Resume Existing Workflow

**This step is only executed if user chose to resume/continue a workflow from Step 0.**

1. **Load the selected state file:**
   Read `.claude/state/workflow-{selected-timestamp}.json`

2. **Determine exact position:**
   - If `current_phase == "discovery"` AND `user_approved == false`:
     → User is at Checkpoint 1 (needs to review/approve Discovery)
   - If `current_phase == "exploration"` AND `selected_ids` is empty:
     → User is at Checkpoint 2 (needs to select concepts)
   - If `current_phase == "design"` AND `status == "complete"`:
     → User is at Checkpoint 3 (needs to review prototype)
   - If `current_phase == "validation"` AND `status == "complete"`:
     → User is at Checkpoint 4 (needs to review validation materials)

3. **Load workflow guide:**
   Read `.claude/workflows/design-centered.md`

4. **Jump to the appropriate checkpoint:**
   - Don't show welcome message again
   - Don't regenerate artifacts that exist
   - Present the user with the checkpoint decision they need to make
   - Example: "Welcome back! You were reviewing design concepts. Here are the 4 concepts I generated earlier..."

5. **Continue workflow from that point**

**After resuming, skip to Step 4 (Execute Workflow) starting from the current phase.**

---

### Step 3: Load Workflow Guide

Read and follow the complete workflow from `.claude/workflows/design-centered.md`

### Step 4: Present Welcome Message

**ALWAYS show this welcome message** - whether the product idea came from $ARGUMENTS or from asking the user:

```
Design-Centered Product Workflow

I'll guide you through a design-first approach to building a product prototype.

We'll go through 4 phases:
1. Discovery - User research, stakeholder mapping, competitive context (iterative)
2. Exploration - Generate and evaluate design concepts (iterative)
3. Design - Create a working Next.js prototype
4. Validation - Stakeholder alignment materials and test scenarios

Each phase has checkpoints where you can review and provide feedback.

Building: "{problem_statement}"
```

### Step 5: Execute Workflow

**After showing the welcome message, start Discovery immediately** - proceed without waiting for confirmation.

**NOTE:** If resuming from Step 2, start from the current phase instead of Discovery.

Follow the workflow exactly as described in `.claude/workflows/design-centered.md`:

**Phase 1: Discovery**
- Read and follow `.claude/prompts/discovery-phase.md`
- Take on the Product & UX Researcher role
- Create PRD, user research, stakeholder map, and competitive landscape
- Present for user approval (Checkpoint 1)
- Allow up to 3 iterations based on feedback
- Save state after completion

**Phase 2: Exploration**
- Read and follow `.claude/prompts/exploration-explorer.md` and `.claude/prompts/exploration-critique.md`
- Alternate between Design Explorer and Design Critic roles
- Generate 3-5 diverse design concepts (text-only, no code)
- Critique each concept on 5 dimensions
- Iterate until 3+ concepts score ≥7.5/10 (max 3 iterations)
- Present concepts for user selection (Checkpoint 2)
- Save state after completion

**Phase 3: Design**
- Read and follow `.claude/prompts/design-phase.md`
- Take on the Full-Stack Developer role
- Generate complete Next.js prototype in `output/{project-name}/`
- Create 12+ files with working code
- Present prototype for user review (Checkpoint 3)
- Allow revisions if requested
- Save state after completion

**Phase 4: Validation**
- Read and follow `.claude/prompts/validation-phase.md`
- Take on the Product & UX Researcher role
- Generate stakeholder alignment summary in `output/{project-name}/STAKEHOLDER_ALIGNMENT.md`
- Generate test scenarios in `output/{project-name}/TEST_SCENARIOS.md`
- Present validation materials for user review (Checkpoint 4)
- Allow revisions if requested
- Save state after completion

### Step 6: Save State After Each Phase

After completing each phase, update the state file with:
- Current phase status
- Completed checkpoints
- Phase-specific data (iterations, concepts, prototype path, validation paths)
- `last_updated` timestamp (current time in ISO-8601 format)

## Important Notes

- **Be conversational and guided** - This is an interactive workflow with checkpoints
- **Allow iteration** - Users can provide feedback at each checkpoint
- **Follow role prompts exactly** - Each phase has specific instructions
- **Save state consistently** - Enables resuming if conversation is interrupted
- **No shortcuts** - Complete all phases unless user explicitly skips
- **Resume is invisible when not needed** - If no existing workflows, behave exactly like before
- **Focus on design, not state** - Don't mention "state files" or "resume capability" in user-facing messages unless resuming

## Examples

**With argument:**
```
/design-flow habit tracking app
```
→ Start with "habit tracking app" as problem statement

**Without argument:**
```
/design-flow
```
→ Ask user what they want to build

**Complex idea:**
```
/design-flow tool to help apartment hunters compare listings and make decisions
```
→ Start with full description as problem statement

---

## Helper Functions

### Relative Time Display

When showing workflow timestamps to users, convert to human-readable format:

```
Time difference calculation:
- Less than 1 hour: "X minutes ago"
- Less than 24 hours: "X hours ago"
- Less than 7 days: "X days ago"
- Less than 30 days: "X weeks ago"
- Else: "on {date}"

Examples:
- Started: 2026-01-17T15:30:00Z (current: 2026-01-17T16:00:00Z) → "30 minutes ago"
- Started: 2026-01-16T15:30:00Z (current: 2026-01-17T16:00:00Z) → "1 day ago"
```

### Phase Display Icons

When showing workflow status, use these visual indicators:
- Discovery phase: ⏸️ (if waiting for approval)
- Exploration phase: 🎨 (if concepts generated)
- Design phase: 💻 (if prototype exists)
- Validation phase: ✅ (if validation complete)

### State File Naming

Always use this format for consistency:
```
workflow-YYYY-MM-DDTHH-MM-SSZ.json

Example: workflow-2026-01-17T15-30-45Z.json
```

Use ISO-8601 format with hyphens instead of colons to avoid filesystem issues.
