# Design-Centered Product Workflow

**Version:** 2.0
**For:** Claude Code
**Purpose:** Guide users through a design-first product development process from discovery to validated prototype

---

## Overview

This is a conversational workflow where you (Claude) take on different roles to guide the user through:
1. **Discovery Phase** - Deep user research, stakeholder mapping, and competitive context
2. **Exploration Phase** - Generate and critique design concepts
3. **Design Phase** - Create working Next.js prototype
4. **Validation Phase** - Generate stakeholder alignment materials and test scenarios

Each phase has iterative refinement loops and human-in-the-loop (HITL) checkpoints.

---

## State Management

### State File Location
`.claude/state/workflow-{timestamp}.json`

### State Schema
```json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "problem_statement": "User's original input",
  "current_phase": "discovery|exploration|design|validation|complete",
  "phase_data": {
    "discovery": {
      "iteration": 1,
      "prd": "...",
      "prd_version": 1,
      "personas": "...",
      "user_journeys": "...",
      "jobs_to_be_done": "...",
      "user_needs": "...",
      "stakeholder_map": "...",
      "competitive_landscape": "...",
      "competitive_skipped": false,
      "pm_approved": false,
      "user_approved": false,
      "iteration_history": [
        {
          "iteration": 1,
          "timestamp": "2024-01-15T10:35:00Z",
          "user_action": "revise",
          "user_feedback": "Personas feel too generic, need more specific pain points",
          "changes_made": "Added specific pain points and realistic quotes to each persona"
        }
      ]
    },
    "exploration": {
      "iteration": 1,
      "concepts": [
        {
          "id": "sol_1_1",
          "name": "Concept Name",
          "description": "...",
          "score": 8.5,
          "critique": "..."
        }
      ],
      "selected_ids": [],
      "user_feedback": "",
      "iteration_history": [
        {
          "iteration": 1,
          "timestamp": "2024-01-15T11:00:00Z",
          "user_action": "retry",
          "user_feedback": "Concepts should explore more diverse interaction patterns",
          "concepts_replaced": ["sol_1_1", "sol_1_3"],
          "new_concepts": ["sol_2_1", "sol_2_2"],
          "reason": "Low scores on conceptual clarity"
        }
      ]
    },
    "design": {
      "prototype_path": "output/project-name",
      "file_count": 0,
      "status": "pending|in_progress|complete|failed",
      "iteration_history": [
        {
          "iteration": 1,
          "timestamp": "2024-01-15T11:30:00Z",
          "user_action": "revise",
          "user_feedback": "Dashboard should show 6 items per row instead of 3",
          "files_changed": ["app/page.tsx", "lib/utils.ts"],
          "changes_made": "Updated grid layout from 3 to 6 columns"
        }
      ]
    },
    "validation": {
      "status": "pending|in_progress|complete|skipped",
      "alignment_summary_path": "...",
      "test_scenarios_path": "...",
      "iteration_history": []
    }
  },
  "checkpoints_completed": []
}
```

### State Operations

**Load State:**
```
Read .claude/state/workflow-*.json (latest file)
```

**Save State:**
```
Write .claude/state/workflow-{current-timestamp}.json
```

**Update State:**
After each significant action (PRD update, concept generation, checkpoint approval)

---

## Workflow Execution

### Initialization

When user requests workflow (e.g., "start design workflow for [problem]"):

1. **Create initial state**
2. **Welcome message:**

```
Design-Centered Product Workflow

I'll guide you through a design-first approach to building a product prototype.

We'll go through 4 phases:
1. Discovery - User research, stakeholder mapping, competitive context (iterative)
2. Exploration - Generate and evaluate design concepts (iterative)
3. Design - Create a working Next.js prototype
4. Validation - Stakeholder alignment materials and test scenarios

Each phase has checkpoints where you can review and provide feedback.

Building: "{problem}"
```

3. **Proceed to Phase 1** - Start Discovery immediately

---

## PHASE 1: Discovery (Iterative)

**Goal:** Develop deep understanding of users, stakeholders, and competitive context through iterative refinement

**Max Iterations:** 3
**Roles:** Product & UX Researcher

### Step 1.1: Create PRD

**Action:** Take on Product & UX Researcher role

**Guidance:** Read `.claude/prompts/discovery-phase.md`

**Output:** Create comprehensive PRD with:
- Problem Statement
- Target Users
- Core Objectives
- Key Features (5-8 features)
- Success Metrics
- Constraints & Assumptions

**Format:**
```markdown
# Product Requirements Document (v{version})

## Problem Statement
[Clear problem description]

## Target Users
[Who will use this?]

## Core Objectives
1. [Objective 1]
2. [Objective 2]
...

## Key Features
1. **[Feature Name]**: [Description]
2. **[Feature Name]**: [Description]
...

## Success Metrics
- [Metric 1]
- [Metric 2]

## Constraints
- [Constraint 1]
- [Constraint 2]
```

**Save:** Update state with PRD, increment version

**Present to User:**
```
📋 Product Requirements Document (v{version})

[Show PRD]

Does this PRD accurately capture what you want to build?

Options:
- **approve** - Move to user research
- **[feedback]** - Provide specific changes you'd like
```

### Step 1.2: Handle User Feedback

**If user provides feedback:**
- Increment PRD version
- Revise PRD with feedback
- **Save iteration history:**
  - Add entry to `phase_data.discovery.iteration_history[]` with:
    - `iteration`: current iteration number
    - `timestamp`: current timestamp
    - `user_action`: "revise"
    - `user_feedback`: exact user feedback text
    - `changes_made`: summary of what was changed in response
- Return to Step 1.1
- Max 3 iterations

**If user approves:**
- Mark PRD as approved
- Proceed to Step 1.3

### Step 1.3: User Research

**Action:** Continue in Product & UX Researcher role

**Guidance:** Continue following `.claude/prompts/discovery-phase.md`

**Input:** Approved PRD

**Output:** Create structured research:

**Format:**
```markdown
## PERSONAS

**Persona 1: [Name/Role]**
- Demographics: [Age, role, context]
- Goals: [What they want to achieve]
- Pain Points: [Current frustrations]
- Tech Comfort: [Skill level]

**Persona 2: [Name/Role]**
[Same structure]

**Persona 3: [Name/Role]** (if applicable)
[Same structure]

---

## USER JOURNEYS

**Journey 1: [Persona Name] - [Goal]**
1. [Step 1]: [Action] → [Thought/Feeling]
2. [Step 2]: [Action] → [Thought/Feeling]
...

**Journey 2: [Persona Name] - [Goal]**
[Same structure]

---

## JOBS TO BE DONE

When [situation], I want to [motivation], so I can [outcome].

1. **[Job 1]**: When [situation], I want to [motivation], so I can [outcome]
2. **[Job 2]**: [Same structure]
...

---

## USER NEEDS (Ranked)

1. **[Need 1]** - [Why important]
2. **[Need 2]** - [Why important]
3. **[Need 3]** - [Why important]
...

---

## ASSUMPTIONS

**User Behavior:**
- [Assumption 1]
- [Assumption 2]

**Technical:**
- [Assumption 1]
- [Assumption 2]

**Business:**
- [Assumption 1]
- [Assumption 2]
```

**Save:** Update state with understanding data

### Step 1.4: Stakeholder Map

**Action:** Identify all stakeholders beyond end users

**Output:** Create stakeholder map:

**Format:**
```markdown
## STAKEHOLDER MAP

| Stakeholder | What They Care About | Success Looks Like |
|-------------|---------------------|-------------------|
| End Users | [Primary concerns] | [Success criteria] |
| Buyers/Decision Makers | [Primary concerns] | [Success criteria] |
| IT/Security | [Primary concerns] | [Success criteria] |
| Support/Operations | [Primary concerns] | [Success criteria] |
| Executives/Leadership | [Primary concerns] | [Success criteria] |

**Key Insight:** [1-2 sentences on stakeholder alignment challenges or opportunities]
```

**Guidelines:**
- Include 3-5 stakeholder groups relevant to this product
- Remove rows that don't apply (e.g., no IT stakeholder for consumer app)
- Focus on stakeholders who influence adoption, purchase, or success

**Save:** Update state with stakeholder map

### Step 1.5: Competitive Landscape

**Action:** Gather competitive context

**Present to User:**
```
Before we explore design concepts, let's understand what exists today.

What existing solutions might your users consider for this problem?

Options:
- **Name 2-3** competitors or alternatives (e.g., "Competitor A, spreadsheets, manual process")
- **search** - I'll research and you validate
- **skip** - Move on without competitive analysis
```

**If user provides names:**
- Structure their input into competitive landscape table
- Use WebFetch to gather additional context if URLs provided
- Ask clarifying questions if needed

**If user says "search":**
- Use WebSearch to find competitors based on problem statement
- Present findings for user validation
- User can add/remove/correct

**If user says "skip":**
- Mark competitive_skipped: true in state
- Proceed to checkpoint

**Output Format (if not skipped):**
```markdown
## COMPETITIVE LANDSCAPE

| Solution | Type | Strengths | Weaknesses | Our Opportunity |
|----------|------|-----------|------------|-----------------|
| [Name] | Competitor | [What they do well] | [Gaps/limitations] | [How we differentiate] |
| [Name] | Adjacent tool | [What they do well] | [Gaps/limitations] | [How we differentiate] |
| [Name] | Workaround | [What they do well] | [Gaps/limitations] | [How we differentiate] |

**Key Insight:** [1-2 sentences on competitive positioning or market gap]
```

**Save:** Update state with competitive landscape (or mark as skipped)

### Step 1.6: Review Discovery

**Action:** Internal review of all discovery artifacts

**Review:** Check alignment across all artifacts

**Internal Check:**
- Do personas match target users in PRD?
- Do journeys address key features?
- Do needs align with objectives?
- Does stakeholder map cover relevant parties?
- Does competitive analysis inform differentiation?

### HITL Checkpoint 1: Approve Discovery

**Present:**
```
================================================================================
CHECKPOINT 1: Discovery Phase Complete
================================================================================

ARTIFACTS CREATED:
✅ Product Requirements Document (v{version})
✅ {N} User Personas
✅ {N} User Journey Maps
✅ Jobs To Be Done Framework
✅ Ranked User Needs
✅ Key Assumptions
✅ Stakeholder Map
{✅ Competitive Landscape | ⏭️ Competitive Landscape (skipped)}

SUMMARY:
{Brief summary of what was learned about users, stakeholders, and market}

================================================================================

Please review all artifacts above.

Your options:
1. **approve** - Move to design exploration
2. **revise [feedback]** - Make changes (e.g., "revise the personas feel too generic")

Your decision:
```

**Handle Response:**
- `approve` → Save checkpoint, proceed to Phase 2
- `revise [feedback]` → Return to Step 1.1 with feedback
- Invalid → Ask again

---

## PHASE 2: Exploration (Iterative)

**Goal:** Generate diverse design concepts and refine until quality threshold met

**Max Iterations:** 3
**Quality Threshold:** ≥3 concepts scoring ≥7.5/10
**Roles:** Explorer Agent, Critique Agent

### Step 2.1: Explorer Agent - Generate Design Concepts

**Action:** Take on UX Designer role

**Guidance:** Read `.claude/prompts/exploration-explorer.md`

**Input:** PRD, Discovery data (personas, journeys, needs, stakeholders, competitive), previous concepts (if retry)

**Target:** Generate 3-5 distinct design concepts

**Output Format:**
```markdown
SOLUTION 1: [Descriptive Name]

WHY THIS APPROACH:
[Explain the design philosophy and why it addresses user needs]

TRADE-OFFS:
**Pros:**
- [Advantage 1]
- [Advantage 2]

**Cons:**
- [Limitation 1]
- [Limitation 2]

CODE DEMONSTRATION:
```typescript
// Show key interaction pattern or data structure
[Working code example - 20-50 lines]
```

---

SOLUTION 2: [Different Approach]
[Same structure]

...
```

**Guidelines:**
- Each concept should take a **different approach**
- Focus on interaction patterns, not visual design
- Code should demonstrate the concept, not be complete
- Address different user needs or personas

**Save:** Add concepts to state

**Present concepts to user, then immediately proceed to critique.**

### Step 2.2: Critique Agent - Evaluate Concepts

**Action:** Take on UX Critic role

**IMPORTANT:** After presenting concepts in Step 2.1, immediately proceed to critique them. Do not wait for user input.

**Guidance:** Read `.claude/prompts/exploration-critique.md`

**Evaluate Each Concept On:**

1. **Information Architecture** (0-10)
   - How well is information organized?
   - Is navigation intuitive?

2. **Interaction Pattern** (0-10)
   - Are interactions familiar and learnable?
   - Does it match user mental models?

3. **Cognitive Load** (0-10)
   - How much thinking is required?
   - Is it simple to understand?

4. **User Flow Alignment** (0-10)
   - Does it support the user journeys?
   - Are key tasks easy?

5. **Visual Clarity** (0-10)
   - Is important information prominent?
   - Is the hierarchy clear?

**Output Format:**
```markdown
CRITIQUE: SOLUTION 1

Information Architecture: 8/10
[Specific feedback]

Interaction Pattern: 7/10
[Specific feedback]

Cognitive Load: 9/10
[Specific feedback]

User Flow Alignment: 8/10
[Specific feedback]

Visual Clarity: 7/10
[Specific feedback]

OVERALL SCORE: 7.8/10

SUMMARY: [2-3 sentences on strengths and weaknesses]
```

**Save:** Update concepts with scores and critiques

### Step 2.3: Iteration Decision

**Check Quality:**
- Count concepts with score ≥7.5/10
- If ≥3 good concepts → Proceed to HITL
- If <3 good concepts AND iteration < 3 → Return to Step 2.1
- If iteration ≥3 → Proceed to HITL anyway (show best available)

**If Iterating:**
```
🔄 Iteration {N}: Quality check

Current scores: {list scores}
Target: 3+ concepts scoring ≥7.5/10

Generating refined concepts based on critique...
```

### HITL Checkpoint 2: Select Design Direction

**Present:**
```
================================================================================
🎨 CHECKPOINT 2: Design Exploration Complete
================================================================================

Generated {N} design concepts (Iteration {I}):

---

**SOLUTION 1: {Name}** ⭐ Score: {X}/10

{Description}

**Why this approach:**
{Why}

**Trade-offs:**
{Pros/Cons summary}

[Show code demonstration]

**Critique:**
{Summary of critique}

---

**SOLUTION 2: {Name}** ⭐ Score: {Y}/10
[Same structure]

---
[Repeat for all concepts]

================================================================================

Your options:
1. **Select concepts** - Enter IDs to pursue (e.g., "1,3" or just "2")
2. **retry [feedback]** - Generate completely new concepts with your guidance
3. **refine [ID] [feedback]** - Refine a specific concept

Your choice:
```

**Handle Response:**
- `1,3` or similar → Save selected IDs, proceed to Phase 3
- `retry [feedback]` →
  - Return to Step 2.1 with feedback
  - **Save iteration history:**
    - Add entry to `phase_data.exploration.iteration_history[]` with:
      - `iteration`: current iteration number
      - `timestamp`: current timestamp
      - `user_action`: "retry"
      - `user_feedback`: exact user feedback text
      - `concepts_replaced`: list of concept IDs being replaced
      - `new_concepts`: list of new concept IDs (filled after regeneration)
      - `reason`: reason for replacement (e.g., "Low scores", "User requested different approach")
- `refine [ID] [feedback]` →
  - Regenerate specific concept with feedback
  - **Save iteration history:**
    - Add entry to `phase_data.exploration.iteration_history[]` with:
      - `iteration`: current iteration number
      - `timestamp`: current timestamp
      - `user_action`: "refine"
      - `user_feedback`: exact user feedback text
      - `concept_id`: ID of concept being refined
      - `changes_made`: summary of changes made to the concept
- Invalid → Ask again

---

## PHASE 3: Design (Single Pass with Validation)

**Goal:** Generate complete, runnable Next.js prototype based on selected concepts

**Roles:** Design Agent (Full-Stack Developer)

### Step 3.1: Design Agent - Generate Prototype

**Action:** Take on Full-Stack Developer role

**Guidance:** Read `.claude/prompts/design-phase.md`

**Input:**
- Selected concept(s)
- All understanding data
- PRD

**Output:** Create 12+ files in `output/{project-name}/`

**Required Files:**
1. package.json
2. tsconfig.json
3. next.config.js
4. tailwind.config.ts
5. postcss.config.js
6. app/layout.tsx
7. app/page.tsx
8. app/globals.css
9. lib/types.ts
10. lib/mockData.ts
11. lib/utils.ts

**Note:** README.md and DESIGN_PROCESS.md will be generated AFTER user approves the prototype (post-Checkpoint 3)

**Process:**
1. Generate project name from problem statement (kebab-case)
2. Create each file using Write tool
3. Validate after creation (see Step 3.2)
4. Save state with prototype path and file count

**File Creation:**
```
For each file:
  Write(
    file_path: "output/{project-name}/{file-path}",
    content: "{complete-file-content}"
  )
```

**Show Progress:**
```
📁 Creating prototype files...
  ✅ package.json
  ✅ tsconfig.json
  ✅ next.config.js
  ✅ app/layout.tsx
  ✅ app/page.tsx
  ...
```

### Step 3.2: Validation

**After creating all files, validate:**

**Check 1: Required Files Present**
- All 12 required files exist
- File paths are correct

**Check 2: App Router Compliance**
```
Read output/{project-name}/app/layout.tsx

Verify:
- ✅ Uses children prop (not Component/pageProps)
- ✅ Imports './globals.css' (not '../styles/globals.css')
- ✅ Has <html> and <body> tags
- ✅ Has metadata export
```

**Check 3: Type Safety**
```
Read output/{project-name}/lib/types.ts

Verify:
- ✅ Exports main entity interfaces
- ✅ All types are complete (not just type aliases)

Read output/{project-name}/lib/mockData.ts

Verify:
- ✅ Imports from './types'
- ✅ Data matches type structure
```

**Check 4: Import Validity**
```
Read output/{project-name}/app/page.tsx

Verify:
- ✅ Imports from '@/lib/types', '@/lib/mockData', '@/lib/utils'
- ✅ Does NOT import from '@/components/ui/' (uses inline components)
```

**If Validation Fails:**
- Show specific errors
- Offer to regenerate failing files
- Max 2 regeneration attempts

### HITL Checkpoint 3: Approve Prototype

**Present:**
```
================================================================================
✅ CHECKPOINT 3: Prototype Generated
================================================================================

Created {N} files in: output/{project-name}/

FILE SUMMARY:
📦 Configuration: package.json, tsconfig.json, next.config.js, etc.
🎨 Application: app/layout.tsx, app/page.tsx, app/globals.css
📚 Types & Data: lib/types.ts, lib/mockData.ts, lib/utils.ts

VALIDATION:
✅ All required files present
✅ App Router pattern (Next.js 13+)
✅ Type safety configured
✅ Imports validated

TO RUN THE PROTOTYPE:
  cd output/{project-name}
  npm install
  npm run dev
  # Open http://localhost:3000

================================================================================

Please test the prototype and review the generated code.

Your options:
1. **approve** - Move to validation phase
2. **approve with no validation** - Complete workflow without validation materials (saves tokens during testing)
3. **revise [feedback]** - Request changes
4. **restart [phase]** - Restart from phase 1, 2, or 3

Your decision:
```

**Handle Response:**
- `approve` → Generate documentation (Step 3.3), save checkpoint, proceed to Phase 4
- `approve with no validation` → Generate documentation (Step 3.3), save checkpoint, mark validation as skipped, jump to Completion
- `revise [feedback]` →
  - Regenerate prototype with feedback
  - **Save iteration history:**
    - Add entry to `phase_data.design.iteration_history[]` with:
      - `iteration`: current iteration number
      - `timestamp`: current timestamp
      - `user_action`: "revise"
      - `user_feedback`: exact user feedback text
      - `files_changed`: list of files modified
      - `changes_made`: summary of what was changed in response
  - Return to Checkpoint 3 (do not generate docs until approved)
- `restart 1` → Clear state, restart from Discovery
- `restart 2` → Keep discovery, restart Exploration
- `restart 3` → Keep concepts, regenerate prototype

### Step 3.3: Generate Documentation (Post-Approval)

**ONLY execute this step after user approves at Checkpoint 3 (either "approve" or "approve with no validation")**

**This ensures documentation captures all prototype iterations.**

1. **Generate README.md:**
```
Write(
  file_path: "output/{project-name}/README.md",
  content: {setup and running instructions}
)
```

**README.md should include:**
- Project name and description
- Prerequisites (Node.js version)
- Installation steps (`npm install`)
- How to run (`npm run dev`)
- Project structure overview
- Technology stack

2. **Generate DESIGN_PROCESS.md:**
```
Write(
  file_path: "output/{project-name}/DESIGN_PROCESS.md",
  content: {full design documentation}
)
```

**DESIGN_PROCESS.md should include:**
- Original problem statement
- Complete PRD
- User research summary (personas, journeys, JTBD, needs)
- Stakeholder map
- Competitive landscape (if not skipped)
- **Discovery iteration history** (if user requested revisions)
- All explored concepts with scores and critiques
- Selected concept(s) and selection rationale
- **Exploration iteration history** (if concepts were regenerated)
- Prototype features and screens
- **Prototype iteration history** (if user requested changes)
- How prototype addresses user needs (map features to needs)

**Data Source:** Read complete workflow state from `.claude/state/workflow-{timestamp}.json` to include all iteration_history arrays from each phase.

3. **Confirm documentation generated:**
```
✅ Documentation generated:
   - README.md (setup instructions)
   - DESIGN_PROCESS.md (complete design journey)
```

---

## PHASE 4: Validation

**Goal:** Generate stakeholder alignment materials and test scenarios for validation

**Roles:** Product & UX Researcher

### Step 4.1: Generate Stakeholder Alignment Summary

**Action:** Create stakeholder alignment document

**Guidance:** Read `.claude/prompts/validation-phase.md`

**Input:** All previous artifacts (PRD, discovery data, selected concepts, prototype)

**Output:** Create alignment summary in `output/{project-name}/STAKEHOLDER_ALIGNMENT.md`:

**Format:**
```markdown
# [Product Name] - Stakeholder Alignment Summary

## The Problem

**Who has this problem:**
[Target users from PRD]

**Business impact:**
[Why this matters - from stakeholder map]

**Current state:**
[How users handle this today - from user journeys and competitive landscape]

---

## Our Approach

**Selected concept:** [Name of selected concept]

**Why this approach:**
[Key rationale from concept selection - address why this over alternatives]

**Key design decisions:**
| Decision | Choice | Rationale |
|----------|--------|-----------|
| [Decision area] | [What we chose] | [Why - tied to user needs/stakeholder concerns] |
| [Decision area] | [What we chose] | [Why] |
| [Decision area] | [What we chose] | [Why] |

---

## The Prototype

**Key screens:**

### [Screen 1 Name]
[Description of what this screen does and key elements]

### [Screen 2 Name]
[Description]

### [Screen 3 Name]
[Description]

**User flow:**
1. [Step 1] → [What user sees/does]
2. [Step 2] → [What user sees/does]
3. [Step 3] → [What user sees/does]
[Continue for main flow]

---

## Stakeholder Considerations

| Stakeholder | Their Concern | How We Address It |
|-------------|---------------|-------------------|
| [Stakeholder 1] | [What they care about] | [How prototype/approach addresses this] |
| [Stakeholder 2] | [What they care about] | [How prototype/approach addresses this] |
| [Stakeholder 3] | [What they care about] | [How prototype/approach addresses this] |

---

## Open Questions

- [Question 1 that needs stakeholder input]
- [Question 2 that needs validation]
- [Question 3 for discussion]

---

## Next Steps

1. [Recommended next action]
2. [Recommended next action]
3. [Recommended next action]
```

**Save:** Write file to `output/{project-name}/STAKEHOLDER_ALIGNMENT.md`

### Step 4.2: Generate Test Scenarios

**Action:** Create usability test scenarios

**Output:** Create test scenarios in `output/{project-name}/TEST_SCENARIOS.md`:

**Format:**
```markdown
# [Product Name] - Test Scenarios

## Overview

These scenarios are designed to validate the prototype with real users.
Each scenario maps to a key user need or job to be done.

---

## Scenario 1: [Primary Task Name]

**Persona:** [Persona name from discovery]
**Job to be done:** [JTBD this validates]
**User need addressed:** [Need from ranked list]

**Setup:**
[Any context or starting state needed]

**Task:**
"[Exact instruction to give the user - in quotes]"

**Steps to observe:**
1. [What user should do first]
2. [What user should do next]
3. [What user should do to complete]

**Success criteria:**
- [ ] Completes task without assistance
- [ ] Completes in under [X] minutes
- [ ] [Additional success criteria]

**Watch for:**
- [Potential confusion point]
- [Potential delight point]
- [Questions they might ask]

---

## Scenario 2: [Secondary Task Name]

**Persona:** [Different persona if applicable]
**Job to be done:** [JTBD this validates]
**User need addressed:** [Need from ranked list]

**Setup:**
[Context]

**Task:**
"[Exact instruction]"

**Steps to observe:**
1. [Step]
2. [Step]
3. [Step]

**Success criteria:**
- [ ] [Criteria]
- [ ] [Criteria]

**Watch for:**
- [Observation point]
- [Observation point]

---

## Scenario 3: [Edge Case or Secondary Flow]

[Same structure]

---

## Post-Test Questions

Ask after all scenarios:

1. What was your overall impression?
2. What was most confusing?
3. What did you like most?
4. What's missing that you expected to see?
5. Would you use this? Why or why not?

---

## Notes for Facilitator

- [Tip for running the test]
- [What to have ready]
- [Common issues to anticipate]
```

**Save:** Write file to `output/{project-name}/TEST_SCENARIOS.md`

### HITL Checkpoint 4: Approve Validation Materials

**Present:**
```
================================================================================
CHECKPOINT 4: Validation Phase Complete
================================================================================

ARTIFACTS CREATED:
✅ Stakeholder Alignment Summary (STAKEHOLDER_ALIGNMENT.md)
✅ Test Scenarios (TEST_SCENARIOS.md)

FILES GENERATED:
output/{project-name}/STAKEHOLDER_ALIGNMENT.md
output/{project-name}/TEST_SCENARIOS.md

These materials are ready for:
- Presenting to stakeholders for alignment
- Running usability tests with target users
- Guiding next iteration of design

================================================================================

Your options:
1. **approve** - Complete workflow
2. **revise [feedback]** - Request changes to validation materials

Your decision:
```

**Handle Response:**
- `approve` → Mark complete, save final state, show completion summary
- `revise [feedback]` →
  - Regenerate validation materials with feedback
  - **Save iteration history:**
    - Add entry to `phase_data.validation.iteration_history[]` with:
      - `iteration`: current iteration number
      - `timestamp`: current timestamp
      - `user_action`: "revise"
      - `user_feedback`: exact user feedback text
      - `files_changed`: list of files modified (STAKEHOLDER_ALIGNMENT.md, TEST_SCENARIOS.md)
      - `changes_made`: summary of what was changed in response

---

## Completion

**When workflow is approved (with or without validation):**

1. **Save final state** with status: "complete"
2. **Show summary:**

**If validation was completed:**
```
DESIGN-CENTERED WORKFLOW COMPLETE

DELIVERABLES:

Discovery:
- Product Requirements Document
- User Research (Personas, Journeys, JTBD, Needs)
- Stakeholder Map
- Competitive Landscape (if completed)

Exploration:
- {N} Design Concepts (Selected: {IDs})
- Critique Evaluations

Design:
- Working Next.js Prototype ({N} files)

Validation:
- Stakeholder Alignment Summary
- Test Scenarios

PROJECT LOCATION:
output/{project-name}/

DOCUMENTATION:
- README.md (setup instructions)
- DESIGN_PROCESS.md (complete design journey with all iterations)
- STAKEHOLDER_ALIGNMENT.md (for stakeholder presentations)
- TEST_SCENARIOS.md (for usability testing)

NEXT STEPS:
1. Run the prototype: cd output/{project-name} && npm install && npm run dev
2. Present to stakeholders using STAKEHOLDER_ALIGNMENT.md
3. Conduct usability tests using TEST_SCENARIOS.md
4. Iterate based on feedback
5. Proceed to high-fidelity design
```

**If validation was skipped:**
```
DESIGN-CENTERED WORKFLOW COMPLETE

DELIVERABLES:

Discovery:
- Product Requirements Document
- User Research (Personas, Journeys, JTBD, Needs)
- Stakeholder Map
- Competitive Landscape (if completed)

Exploration:
- {N} Design Concepts (Selected: {IDs})
- Critique Evaluations

Design:
- Working Next.js Prototype ({N} files)

Validation:
- Skipped (tokens saved during testing)

PROJECT LOCATION:
output/{project-name}/

DOCUMENTATION:
- README.md (setup instructions)
- DESIGN_PROCESS.md (complete design journey with all iterations)

NEXT STEPS:
1. Run the prototype: cd output/{project-name} && npm install && npm run dev
2. Iterate based on feedback
3. Proceed to high-fidelity design
```

---

## Resuming Workflow

**If conversation interrupted:**

1. **Check for existing state:**
   ```
   Read .claude/state/workflow-*.json (latest)
   ```

2. **If state found:**
   ```
   Found saved workflow state from {timestamp}

   Status:
   - Problem: {problem}
   - Current Phase: {phase}
   - Last Action: {last_action}

   Would you like to:
   1. **continue** - Resume from where we left off
   2. **restart** - Start fresh
   3. **review** - Show what we've created so far
   ```

3. **Resume from saved phase**

---

## Error Handling

**If file generation fails:**
- Show specific error
- Explain what went wrong
- Offer to retry with adjustments
- Max 2 retry attempts
- Option to skip and continue

**If user input unclear:**
- Ask clarifying question
- Provide examples
- Don't assume - always confirm

**If iteration limit reached:**
- Explain what was attempted
- Show best results so far
- Ask if user wants to proceed or restart

---

## Tips for Execution

1. **Be conversational** - This is a chat, not a script
2. **Show progress** - Use emojis, progress indicators
3. **Explain decisions** - Help user understand why you're doing things
4. **Adapt to feedback** - Don't rigidly follow if user wants something different
5. **Save state frequently** - After every major action
6. **Validate assumptions** - Ask if unsure about requirements
7. **Celebrate progress** - Acknowledge completed checkpoints

---

## End of Workflow Guide
