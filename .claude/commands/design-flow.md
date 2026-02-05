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

### Step 1: Load Workflow Guide

Read and follow the complete workflow from `.claude/workflows/design-centered.md`

### Step 2: Present Welcome Message

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

### Step 3: Execute Workflow

**After showing the welcome message, start Discovery immediately** - proceed without waiting for confirmation.

Follow the workflow exactly as described in `.claude/workflows/design-centered.md`:

**Phase 1: Discovery**
- Read and follow `.claude/prompts/discovery-phase.md`
- Take on the Product & UX Researcher role
- Create PRD, user research, stakeholder map, and competitive landscape
- Present for user approval (Checkpoint 1)
- Allow up to 3 iterations based on feedback

**Phase 2: Exploration**
- Read and follow `.claude/prompts/exploration-explorer.md` and `.claude/prompts/exploration-critique.md`
- Alternate between Design Explorer and Design Critic roles
- Generate 3-5 diverse design concepts (text-only, no code)
- Critique each concept on 5 dimensions
- Iterate until 3+ concepts score ≥7.5/10 (max 3 iterations)
- Present concepts for user selection (Checkpoint 2)

**Phase 3: Design**
- Read and follow `.claude/prompts/design-phase.md`
- Take on the Full-Stack Developer role
- Generate complete Next.js prototype in `output/{project-name}/`
- Create 12+ files with working code
- Present prototype for user review (Checkpoint 3)
- Allow revisions if requested

**Phase 4: Validation**
- Read and follow `.claude/prompts/validation-phase.md`
- Take on the Product & UX Researcher role
- Generate stakeholder alignment summary in `output/{project-name}/STAKEHOLDER_ALIGNMENT.md`
- Generate test scenarios in `output/{project-name}/TEST_SCENARIOS.md`
- Present validation materials for user review (Checkpoint 4)
- Allow revisions if requested

## Important Notes

- **Be conversational and guided** - This is an interactive workflow with checkpoints
- **Allow iteration** - Users can provide feedback at each checkpoint
- **Follow role prompts exactly** - Each phase has specific instructions
- **No shortcuts** - Complete all phases unless user explicitly skips

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
