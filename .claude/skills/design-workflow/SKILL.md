---
name: design-workflow
description: Start a comprehensive design-centered product development workflow with user research, stakeholder mapping, competitive analysis, concept exploration with critique, prototype generation, and validation materials. Guides through 4 phases - Discovery (PRD, personas, stakeholder map, competitive landscape), Exploration (3-5 design concepts with UX critique), Design (working Next.js prototype with iteration tracking), and Validation (stakeholder alignment summary and usability test scenarios). Use when the user wants to build a new product, feature, or tool and needs a structured design thinking process from research through validated prototype.
---

# Design Workflow Skill

## What This Skill Does

Guides you through a structured design thinking process:
1. **Discovery** - Creates PRD, user research, stakeholder map, and competitive context
2. **Exploration** - Generates and critiques 3-5 design concepts
3. **Design** - Builds a working Next.js prototype
4. **Validation** - Generates stakeholder alignment materials and test scenarios

---

## When to Use This Skill

Use this skill when the user:
- Wants to build a new product or feature
- Needs help understanding user needs
- Wants to explore different design approaches
- Needs a working prototype quickly

---

## When Invoked

Follow these steps:

### 1. Capture the Product Idea

If the user provided an argument (product idea), use that.
If not, ask: "What product or feature would you like to build?"

### 2. Load the Workflow

Read and follow the main workflow guide at `.claude/workflows/design-centered.md`

### 3. Start the Workflow

Present the welcome message:

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

**Proceed immediately to Phase 1 (Discovery)** - no confirmation needed.

### 4. Follow the Workflow

Execute the workflow exactly as described in `.claude/workflows/design-centered.md`:

- **Phase 1:** Take on Product & UX Researcher role (read `.claude/prompts/discovery-phase.md`)
- **Phase 2:** Alternate between Explorer and Critic roles (read `.claude/prompts/exploration-explorer.md` and `.claude/prompts/exploration-critique.md`)
- **Phase 3:** Take on Full-Stack Developer role (read `.claude/prompts/design-phase.md`)
- **Phase 4:** Take on Product & UX Researcher role (read `.claude/prompts/validation-phase.md`)

---

## Examples

**User says:** "I want to build a habit tracking app"
**Response:** Start the workflow with "habit tracking app" as the problem statement

**User says:** "Help me design a tool for freelancers"
**Response:** Start the workflow with "tool for freelancers" as the problem statement

---

## Notes

- The workflow takes 20-40 minutes depending on iterations
- Generates working Next.js prototype in `output/{project-name}/`
- Also generates stakeholder alignment summary and test scenarios

---

## Skill Behavior

This skill is **conversational and guided**:
- Asks for approval at 4 checkpoints (one per phase)
- Allows iteration based on feedback
- Adapts to user needs throughout process
- Maintains full context across phases
