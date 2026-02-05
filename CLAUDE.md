# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Project Overview

This is a **Claude-native conversational workflow** for design-centered product development. It guides users through a structured design thinking process—from understanding user needs to generating working prototypes—entirely through conversation with Claude Code.

**No code to run, no setup required, no API costs.**

## Quick Start

There are three ways to start the workflow:

### Method 1: Slash Command (Recommended)

Explicitly invoke the workflow with:
```
/design-flow [your product idea]
```

Examples:
```
/design-flow habit tracking tool
/design-flow apartment comparison tool
/design-flow meal planning app
```

**Why this is recommended:** Provides immediate, predictable startup with argument support.

### Method 2: Skill (Auto-discovery)

Just describe what you want naturally:
```
I want to build a habit tracking tool
Help me create an apartment comparison tool
```

Claude will automatically recognize your intent and invoke the workflow skill.

**Why use this:** Natural conversation flow without explicit commands.

### Method 3: Direct Workflow Reference

Reference the workflow directly:
```
Follow the design-centered workflow in .claude/workflows/ to help me build [your idea]
```

Example:
```
Follow the design-centered workflow to help me build a habit tracking tool
```

**Why use this:** Maximum control over how the workflow starts.

## How the Workflow Works

The workflow is defined through markdown guide files that Claude reads and follows conversationally:

### File Structure

```
.claude/
├── commands/
│   └── design-flow.md              # Slash command for /design-flow
├── skills/
│   └── design-workflow/
│       └── SKILL.md                # Auto-discovery skill definition
├── workflows/
│   └── design-centered.md          # Main workflow orchestration guide
└── prompts/
    ├── discovery-phase.md          # Product & UX Researcher role prompt (Phase 1)
    ├── exploration-explorer.md     # Design Explorer role prompt (Phase 2)
    ├── exploration-critique.md     # Design Critic role prompt (Phase 2)
    ├── design-phase.md             # Full-Stack Developer role prompt (Phase 3)
    └── validation-phase.md         # Product & UX Researcher role prompt (Phase 4)

output/
└── {project-name}/                 # Generated prototypes and validation materials
```

**About the Slash Command and Skill:**

This workflow is available in two forms:

**Slash Command** (`.claude/commands/design-flow.md`):
- Explicitly invoked with `/design-flow [product idea]`
- Accepts arguments for the product idea
- Provides predictable, immediate startup

**Skill** (`.claude/skills/design-workflow/SKILL.md`):
- Auto-discovered by Claude when you describe wanting to build something
- Triggered by natural language (e.g., "I want to build...")
- Provides conversational workflow initiation

Both methods:
- Automatically capture the product idea
- Load the workflow guide
- Present the welcome message
- Start Phase 1 (Discovery)

### Workflow Phases

#### Phase 1: Discovery

Claude takes on the **Product & UX Researcher** role by reading `.claude/prompts/discovery-phase.md`.

**Creates:**
1. **Product Requirements Document (PRD)**
   - Problem statement
   - Target users
   - Core objectives (3-5)
   - Key features (5-8)
   - Success metrics
   - Constraints
   - Assumptions

2. **User Research**
   - 2-3 detailed personas with demographics, goals, pain points, motivations, quotes
   - User journey maps showing current frustrations (5-8 steps each)
   - Jobs To Be Done framework (3-5 jobs)
   - Prioritized user needs (8-12 needs ranked by importance)
   - Assumptions to validate

3. **Stakeholder Map**
   - 3-5 stakeholder groups (end users, buyers, IT, support, executives)
   - What each stakeholder cares about
   - Success criteria per stakeholder
   - Key insight on alignment challenges

4. **Competitive Landscape** (optional - user can skip)
   - 2-4 existing solutions (competitors, adjacent tools, workarounds)
   - Strengths and weaknesses of each
   - Opportunities for differentiation
   - Key insight on market positioning

**User can provide competitors, ask Claude to search, or skip competitive analysis.**

**Presents all sections together** for user review.

**Checkpoint 1:** User approves or provides feedback for revision.

**Iteration:** Up to 3 iterations to refine discovery based on user feedback.

#### Phase 2: Exploration

Claude alternates between two roles:

**Design Explorer** (reads `.claude/prompts/exploration-explorer.md`):
- Generates 3-5 **distinctly different** design concepts
- Each concept is **text-only** (no code, no ASCII diagrams)
- Describes:
  - **WHY THIS APPROACH** - Philosophy, rationale, user needs addressed
  - **HOW IT WORKS** - Main screen layout, user flow (5-8 steps), key interactions
  - **TRADE-OFFS** - Specific pros and cons

**Design Critic** (reads `.claude/prompts/exploration-critique.md`):
- Evaluates each concept on 5 dimensions (0-10 scale):
  1. **Conceptual Clarity** - Is the approach well-explained?
  2. **User Need Alignment** - Does it address user research?
  3. **Approach Feasibility** - Can this be built?
  4. **Interaction Soundness** - Does the flow make sense?
  5. **Strategic Differentiation** - Is this novel/interesting?
- Calculates **Overall Score** (average of 5 dimensions)
- Provides specific, constructive feedback

**Iteration Logic:**
- **Target:** ≥3 concepts scoring ≥7.5/10
- **Max iterations:** 3
- If target not met after 3 iterations, proceed anyway with best available

**Checkpoint 2:** User selects which concept(s) to pursue (can select 1 or multiple).

#### Phase 3: Design

Claude takes on the **Full-Stack Developer** role by reading `.claude/prompts/design-phase.md`.

**Generates:**
- Complete Next.js 13+ application with App Router
- 12+ files in proper directory structure
- TypeScript with full type safety
- Tailwind CSS for styling
- Realistic mock data (domain-specific, 10-15+ items)
- All key screens from selected concept(s)
- All interaction states (loading, empty, error, success, hover, selected)
- Production-quality UI
- Working interactions (no TODOs or placeholders)
- **DESIGN_PROCESS.md** - Complete design journey with iteration history

**Output location:** `output/{project-name}/`

**Checkpoint 3:** User reviews prototype and can:
- **approve** - Move to validation phase
- **approve with no validation** - Complete workflow without validation (saves tokens)
- **revise [feedback]** - Request changes to prototype
- **restart [phase]** - Restart from earlier phase

#### Phase 4: Validation (Optional)

Claude takes on the **Product & UX Researcher** role by reading `.claude/prompts/validation-phase.md`.

**Note:** This phase can be skipped using "approve with no validation" at Checkpoint 3 to save tokens during testing/iteration.

**Generates:**
1. **Stakeholder Alignment Summary** (`STAKEHOLDER_ALIGNMENT.md`)
   - Problem statement and business impact
   - Selected approach and rationale
   - Key design decisions with justification
   - Prototype overview (key screens, user flow)
   - How each stakeholder's concerns are addressed
   - Open questions for discussion
   - Risks and mitigations
   - Recommended next steps

2. **Test Scenarios** (`TEST_SCENARIOS.md`)
   - 3-4 usability test scenarios mapped to personas and JTBD
   - Exact task instructions for facilitators
   - Success criteria for each scenario
   - What to observe and watch for
   - Post-test questions
   - Debrief and synthesis templates

**Output location:** `output/{project-name}/`

**Checkpoint 4:** User reviews validation materials and approves or requests revisions.

### Documentation

#### DESIGN_PROCESS.md Generation

**Timing:** Generated AFTER user approves the prototype at Checkpoint 3 (not during initial prototype generation).

This ensures the documentation captures **all prototype iterations**, including any revisions requested by the user.

**Process:**
1. User approves prototype (says "approve" or "approve with no validation")
2. Generates `DESIGN_PROCESS.md` and `README.md`
3. Then proceeds to Phase 4 (Validation) or Completion

**DESIGN_PROCESS.md includes:**
- **Discovery Iterations** - What feedback was given, what changed
- **Exploration Iterations** - Which concepts were rejected/refined and why
- **Prototype Iterations** - What prototype revisions were requested
- **Design Decisions** - Why the selected concept was chosen
- **User Need Mapping** - How each top user need is addressed

This creates a complete audit trail of the design journey.

## Role Prompt Details

### Discovery Phase (`discovery-phase.md`)

**Role:** Product & UX Researcher

**Key principle:** Create PRD, user research, stakeholder map, and competitive context together in one cohesive analysis.

**Output structure:**
- Part A: Product Requirements Document
- Part B: User Research
- Part C: Stakeholder Map
- Part D: Competitive Landscape (optional)
- Presented together, not separately

**Competitive landscape options:**
- User provides 2-3 competitors/alternatives
- User says "search" - Claude researches and user validates
- User says "skip" - proceed without competitive analysis

**Iteration behavior:**
- If user provides feedback, increment PRD version and revise
- Max 3 iterations before proceeding
- Ask clarifying questions if user input is ambiguous

**Quality checklist before presenting:**
- Problem statement is specific and concrete
- 3-5 objectives that are measurable
- 5-8 features focused on user value
- 2-3 distinct personas with realistic details
- 2-3 user journeys with thoughts, feelings, pain points
- 3-5 JTBD statements in proper format
- 8-12 prioritized user needs
- 3-5 stakeholder groups with concerns and success criteria
- Competitive landscape (if not skipped) with differentiation opportunities
- All sections tie together cohesively

### Exploration - Explorer (`exploration-explorer.md`)

**Role:** Design Explorer / UX Designer

**Goal:** Generate 3-5 diverse, high-quality design concepts

**Key principle:** Divergent thinking - explore solution space broadly through clear text descriptions.

**Output format:**
```markdown
## SOLUTION 1: [Descriptive Name]

### WHY THIS APPROACH
[2-3 paragraphs: interaction pattern, user needs prioritized, core UX, differentiation]

### HOW IT WORKS

**Main Screen:**
[Layout, key elements, information hierarchy]

**User Flow:**
1. [User action] → [What happens/what they see]
2. [User action] → [What happens/what they see]
[Continue 5-8 steps]

**Key Interactions:**
- [Primary interaction pattern]
- [Secondary interactions]
- [Novel interaction mechanisms]

### TRADE-OFFS

**Pros:**
- [Specific advantage 1]
- [Specific advantage 2]
- [Specific advantage 3]

**Cons:**
- [Specific limitation 1]
- [Specific limitation 2]
- [Specific limitation 3]
```

**Diversity dimensions to vary across:**
- Interaction pattern (direct manipulation vs. guided, single view vs. multi-step)
- Information architecture (dashboard vs. drill-down, list vs. card vs. table)
- User control (customizable vs. opinionated, advanced vs. simple)
- Persona focus (which user type does this best serve?)

**What NOT to include:**
- ❌ No code demonstrations
- ❌ No ASCII diagrams or wireframes
- ❌ No visual design details (colors, fonts)
- ✅ Only text descriptions of concepts

**Iteration behavior:**
- Keep high-scoring concepts (≥7.5/10)
- Refine or replace low-scoring concepts
- Use critique feedback to improve

### Exploration - Critique (`exploration-critique.md`)

**Role:** Design Critic / Concept Evaluator

**Goal:** Evaluate text-based design concepts to identify strongest strategic approaches.

**What you're evaluating:** Text descriptions of concepts, NOT actual designs or wireframes.

**Evaluation dimensions (0-10 scale):**

1. **Conceptual Clarity (0-10)**
   - Is the approach clearly explained?
   - Can you picture how this would work?
   - Is there enough detail?

2. **User Need Alignment (0-10)**
   - Does it address user needs from research?
   - Which personas does it serve?
   - Does it solve pain points?

3. **Approach Feasibility (0-10)**
   - Can this be built?
   - Are there technical barriers?
   - Does the interaction model make sense?

4. **Interaction Soundness (0-10)**
   - Does the user flow make logical sense?
   - Are interactions intuitive?
   - Any problematic steps?

5. **Strategic Differentiation (0-10)**
   - Is this approach novel?
   - Is differentiation meaningful?
   - Does it explore solution space effectively?

**Output format:**
```markdown
## CRITIQUE: [Concept Name]

### Evaluation Scores

**1. Conceptual Clarity: [X]/10**
[2-3 sentences explaining score with specific examples]

**2. User Need Alignment: [X]/10**
[2-3 sentences with references to personas/journeys/needs]

**3. Approach Feasibility: [X]/10**
[2-3 sentences about implementation considerations]

**4. Interaction Soundness: [X]/10**
[2-3 sentences about flow logic]

**5. Strategic Differentiation: [X]/10**
[2-3 sentences about novelty and meaningfulness]

**OVERALL SCORE: [Average]/10**

### Key Strengths
- [Specific strength with reference to concept description]
- [Specific strength with reference to user research]
- [Specific strength]

### Key Concerns
- [Specific concern with suggestion for improvement]
- [Specific concern]
- [Specific concern]

### Recommendation
[One of: "Strong concept - ready for design phase", "Promising - needs minor clarification", "Interesting but needs rework", "Weak concept - consider replacing"]
```

**Scoring guidance:**
- Be honest and specific
- Reference user research to ground critiques
- Provide actionable suggestions
- Balance rigor and encouragement

**Iteration recommendation:**
- ≥3 concepts scoring ≥7.5/10 → "Ready for user selection"
- 1-2 strong concepts → "Iterate on weaker concepts"
- 0 strong concepts → "Regenerate with focused feedback"

### Design Phase (`design-phase.md`)

**Role:** Full-Stack Developer / Prototype Builder

**Goal:** Generate complete, runnable Next.js prototype embodying selected concept(s).

**Key principle:** Trust Claude's native ability to build modern web apps. Minimal prescriptive instructions.

**Requirements:**
1. **Embody the selected concept** - Implement interaction patterns and flows described
2. **Address user needs** - Solve problems from personas, journeys, JTBD
3. **Immediately runnable** - `npm install && npm run dev` works
4. **Realistic content** - Domain-specific data and copy
5. **Complete implementations** - No TODOs or placeholders
6. **High-fidelity** - Production-quality UI with all states

**What to generate:**
- Modern Next.js 13+ App Router setup
- TypeScript for type safety
- Tailwind CSS for styling
- Realistic mock data matching domain
- All key screens from selected concept
- All interaction states (loading, empty, error, success, hover, selected)
- Complete user flows
- Production-quality UI (spacing, typography, hierarchy)

**Output location:** `output/{project-name}/`

**Minimum files (generated during prototype creation):**
- package.json, tsconfig.json, next.config.js
- tailwind.config.ts, postcss.config.js
- app/layout.tsx, app/page.tsx, app/globals.css
- lib/types.ts, lib/mockData.ts, lib/utils.ts

**Documentation files (generated AFTER user approves at Checkpoint 3):**
- README.md (setup and running instructions)
- DESIGN_PROCESS.md (complete design journey with all iterations)

**After generation, present:**
```markdown
✅ Prototype Generated

Created {N} files in `output/{project-name}/`

**To run:**
```bash
cd output/{project-name}
npm install
npm run dev
```

Open http://localhost:3000

**What this prototype demonstrates:**
[Key features from selected concept]

**Screens/views included:**
[List main screens/views]

**How it addresses user needs:**
[Connection to personas/journeys/needs]
```

### Validation Phase (`validation-phase.md`)

**Role:** Product & UX Researcher

**Goal:** Generate stakeholder alignment materials and test scenarios for validation.

**Key principle:** Transform design process artifacts into materials that enable stakeholder alignment and user validation.

**Stakeholder Alignment Summary includes:**
- Problem statement and business impact
- Selected approach and rationale
- Key design decisions with justification
- Prototype overview (key screens, user flow)
- How each stakeholder's concerns are addressed
- Open questions for discussion
- Risks and mitigations
- Recommended next steps

**Test Scenarios include:**
- 3-4 scenarios mapped to personas and JTBD
- Exact task instructions for facilitators
- Success criteria for each scenario
- What to observe and watch for
- Post-test questions
- Debrief and synthesis templates

**Output files:**
- `output/{project-name}/STAKEHOLDER_ALIGNMENT.md`
- `output/{project-name}/TEST_SCENARIOS.md`

## Customization

### Modifying Role Prompts

Edit files in `.claude/prompts/` to change agent behavior:

**Discovery Phase:**
- Add/remove PRD sections
- Change persona template
- Modify JTBD framework
- Adjust needs prioritization
- Customize stakeholder categories
- Modify competitive analysis format

**Explorer Agent:**
- Change number of concepts (default 3-5)
- Add diversity dimensions
- Modify output format
- Add domain-specific guidance

**Critique Agent:**
- Modify evaluation dimensions
- Change scoring rubrics
- Add criteria (e.g., accessibility)
- Adjust quality threshold

**Design Agent:**
- Specify different frameworks (Vue, React Native)
- Change styling approach (styled-components, CSS modules)
- Add backend integration requirements
- Modify file structure

**Validation Phase:**
- Customize stakeholder alignment summary sections
- Add/remove test scenario fields
- Modify post-test questions
- Add accessibility checklist
- Add risk assessment criteria

### Modifying Workflow Structure

Edit `.claude/workflows/design-centered.md` to change:

**Phase configuration:**
- Add new phases (e.g., competitive analysis, user testing)
- Remove phases
- Reorder phases

**Iteration limits:**
- Change max iterations per phase
- Adjust quality thresholds
- Modify decision logic

**Checkpoints:**
- Add checkpoints within phases
- Change approval criteria
- Add skip options

### Adding New Phases

1. **Create role prompt:** Add `.claude/prompts/new-phase.md`
2. **Update workflow guide:** Add phase to `.claude/workflows/design-centered.md`
3. **Test:** Run workflow with example problem

## Best Practices

### Starting a Workflow

**Be clear and specific:**
```
✅ "Build a tool to help freelancers track billable hours and generate invoices"
❌ "Make something for freelancers"
```

**Provide context upfront:**
```
✅ "Build a mobile app for iOS and Android that helps runners track training progress"
❌ "Build a running app"
```

### During Discovery

**Review carefully:**
- Personas should feel real and distinct
- User needs should align with your vision
- Stakeholder map should cover all relevant parties
- Competitive landscape should inform differentiation
- Don't skip this - it drives all design decisions

**Iterate if needed:**
- Provide specific feedback ("Sarah's persona should focus more on X")
- Don't approve if something feels wrong
- Max 3 iterations, but usually 1-2 is enough

### During Exploration

**Read all concepts before selecting:**
- Each offers different trade-offs
- Consider which serves your target users best
- You can select multiple to combine approaches

**Provide feedback for retry:**
```
✅ "Generate concepts focused more on mobile-first interaction patterns"
❌ "Make it better"
```

### During Design

**Test immediately:**
```bash
cd output/{project-name}
npm install
npm run dev
```

**Provide specific feedback:**
```
✅ "The dashboard view should show 6 items per row instead of 3"
❌ "This doesn't look right"
```

**Remember:**
- Prototypes are starting points, not final products
- Minor fixes are expected
- Focus on validating concept, not pixel perfection

## Troubleshooting

### Workflow Doesn't Start

**Problem:** Claude doesn't follow the workflow when asked

**Solutions:**
1. Check `.claude/workflows/design-centered.md` exists
2. Try: "Read .claude/workflows/design-centered.md and start the workflow"
3. Verify you're using Claude Code (not regular Claude chat)
4. Try restarting Claude Code

### Prototype Has Errors

**Common issues and fixes:**

**"Module not found" errors:**
- Run `npm install` (don't skip this step)
- Check package.json has all dependencies
- Verify import paths use `@/` prefix correctly

**"use client" directive missing:**
- Add `"use client"` at top of files using hooks or event handlers
- Claude should include this, but may miss in some cases

**TypeScript errors:**
- Check lib/types.ts exports match imports
- Verify mock data matches type definitions
- Most errors self-explanatory with stack trace

**Styling issues:**
- Verify tailwind.config.ts content paths include all directories
- Check app/globals.css has @tailwind directives
- Run `npm run dev` to regenerate Tailwind classes

**General debugging:**
1. Check browser console for specific errors
2. Read error messages carefully - they point to exact issues
3. Ask Claude to fix specific errors you encountered
4. Prototypes are starting points - minor fixes expected

### Quality Issues

**Problem:** Generated concepts are too similar or low quality

**Solutions:**
- Provide more specific problem description
- Give feedback at exploration checkpoint: "retry - concepts should explore more diverse interaction patterns"
- Be clear about target users and constraints upfront

**Problem:** Prototype doesn't match selected concept

**Solutions:**
- Check that concept description was detailed enough
- Provide specific feedback: "The prototype should include X feature from the concept"
- Ask Claude to regenerate with more emphasis on specific aspects

## Advanced Usage

### Combining Multiple Concepts

You can select 2+ concepts at Checkpoint 2:
```
Your choice: 1,3
```

Claude will generate a prototype that combines elements from both. This works best when concepts are complementary (e.g., one focuses on data view, another on interaction pattern).

### Restarting from Different Phases

At Checkpoint 3 or 4, you can restart from any phase:
```
Your decision: revise

Which phase to restart from?
1 - Discovery
2 - Exploration
3 - Design (regenerate prototype)
4 - Validation (regenerate validation materials)

Your choice: 2
```

This lets you:
- Go back to exploration with new discovery insights
- Regenerate prototype with different concept
- Regenerate validation materials with updates
- Iterate on specific phase without redoing everything

### Custom Templates

Create custom templates in `.claude/prompts/` for specific domains:

Example: `.claude/prompts/discovery-phase-ecommerce.md`
- Tailored PRD sections for e-commerce
- E-commerce specific personas
- Checkout flow journey templates
- E-commerce JTBD examples
- B2B vs B2C stakeholder maps
- E-commerce competitive landscape template

Then modify workflow guide to use domain-specific prompts.

## Contributing

### Testing Different Domains

Help improve the workflow by testing with:
- Different problem types (B2B vs B2C, mobile vs web)
- Various complexity levels (simple tools vs complex platforms)
- Diverse user types (technical vs non-technical)

Document what works well and what doesn't.

### Enhancing Role Prompts

Improve prompts by:
- Adding more examples
- Clarifying ambiguous instructions
- Adding edge case handling
- Improving evaluation criteria

Submit PRs with:
- Clear description of improvement
- Test results showing benefit
- Updated documentation

### Extending Workflow

Add new capabilities:
- Backend integration templates
- Visual wireframe generation
- Accessibility audit phase
- Performance optimization phase
- Decision log generation

Open issues to discuss before implementing.

## Limitations

**Current constraints:**
- Only tested with Claude Sonnet 4.5
- Generates Next.js prototypes only (not Vue, React Native, etc.)
- Text-only concept descriptions (no visual wireframes)
- Mock data only (no real backend integration)
- English language only
- Requires Claude Pro subscription

**Future enhancements:**
- Multi-framework support
- Visual design generation
- Backend integration
- Multi-language support
- Collaborative workflows

## FAQ

**Q: Can I use this without Claude Code?**
A: No, this workflow is specifically designed for Claude Code's conversational interface and file system access.

**Q: Does this work with other AI assistants?**
A: Not currently. The workflow relies on Claude's specific capabilities and prompt following.

**Q: Can I modify the generated prototype?**
A: Absolutely! The prototype is a starting point. Modify, extend, and iterate as needed.

**Q: How much does this cost?**
A: Just a Claude Pro subscription ($20/month). No additional API costs.

**Q: Can I generate prototypes for mobile apps?**
A: Currently generates Next.js web apps. React Native support could be added by customizing the design agent prompt.

**Q: Is the generated code production-ready?**
A: No, it's a high-fidelity prototype for validation. Requires additional work for production (backend, auth, deployment, etc.).

**Q: Can I share prototypes with stakeholders?**
A: Yes! Deploy to Vercel/Netlify or share the code. Prototypes are fully functional.

**Q: How long does the workflow take?**
A: Typically 20-40 minutes for the full 4-phase process, depending on iteration and complexity.

## License

MIT License - See LICENSE file for details.

## Acknowledgments

- Built for Claude Code by Anthropic
- Inspired by design thinking methodology
- Created to demonstrate AI-assisted design processes
