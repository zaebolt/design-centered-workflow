# Design-Centered Workflow for Claude Code (Proof of concept)

A conversational design-first workflow that runs natively in Claude Code. This workflow guides you through a complete design thinking process—from understanding user needs to generating working prototypes—all through natural conversation with Claude.

📝 **Read the full story:** [From Vibes to Enterprise: Design Intelligence in the Age of AI](https://vpdai.substack.com/p/from-vibes-to-enterprise-design-intelligence)

## 🎯 What is This?

A structured workflow that brings **design thinking methodology** to AI-assisted product development. Instead of jumping straight to code, this workflow ensures you:

1. **Deeply understand** the problem and users
2. **Explore multiple solutions** before committing to one
3. **Generate high-fidelity prototypes** based on validated concepts

## 💡 The Problem This Solves

Most AI coding tools follow a pattern like this:
```
User describes idea → AI writes code → Hope it's right
```

This skips crucial design thinking steps that lead to:
- ❌ Building solutions before understanding the problem
- ❌ Missing important user needs
- ❌ No exploration of alternative approaches
- ❌ Prototypes that don't solve the real problem

## ✨ The Solution

A four-phase conversational workflow that puts design thinking first:

```
1. DISCOVERY              2. EXPLORATION           3. DESIGN                4. VALIDATION
   Product & UX Research     Generate & Critique      Build Prototype          Stakeholder Alignment
   ↓                         ↓                        ↓                        ↓
   PRD + User Research      3-5 Design Concepts      Working Next.js App      Test Scenarios + Docs
```

### Phase 1: Discovery
- **Product Requirements Document** - Problem, users, objectives, features, constraints
- **User Personas** - 2-3 detailed personas with goals, pain points, motivations
- **User Journeys** - Step-by-step maps showing current frustrations
- **Jobs To Be Done** - What users are trying to accomplish
- **Prioritized Needs** - Ranked list of 8-12 user needs
- **Stakeholder Map** - Key stakeholders and their success criteria
- **Competitive Landscape** - Existing solutions and differentiation opportunities (optional)

**Checkpoint 1:** Review and approve/revise discovery artifacts

**Iteration Tracking:** All user feedback and revisions are captured for traceability

### Phase 2: Exploration
- **Design Concepts** - 3-5 diverse approaches (text descriptions)
- **Critique Evaluation** - Each concept scored on 5 UX dimensions
- **Iteration** - Refine until ≥3 concepts score ≥7.5/10

**Checkpoint 2:** Select which concept(s) to pursue

**Iteration Tracking:** All concept iterations and user selections are documented

### Phase 3: Design
- **Working Prototype** - Complete Next.js application
- **Mid-Fidelity UI** - Using Tailwind CSS
- **Realistic Data** - Domain-specific mock data
- **Design Process Documentation** - Complete design journey with iteration history

**Checkpoint 3:** Review prototype and approve/revise.

**Iteration Tracking:** All prototype revisions and changes documented

### Phase 4: Validation (Optional)
- **Stakeholder Alignment Summary** - How the design addresses stakeholder concerns
- **Test Scenarios** - Usability test plans mapped to personas and JTBD
- **Success Criteria** - Measurable validation metrics

**Checkpoint 4:** Review validation materials and approve/revise

**Option to Skip:** Use "approve with no validation" to save tokens during testing this POC.

## 🚀 Getting Started

### Prerequisites
- [Claude Code](https://claude.ai/code) (Claude Pro subscription)
- That's it! No installation, no setup, no API keys.

## 🔌 Installation

Clone this repository:

```bash
git clone https://github.com/zaebolt/design-centered-workflow.git
cd design-centered-workflow
```

Open the directory in Claude Code and the workflow will be available immediately.

## 📖 Usage

> **⚠️ Token Usage Warning:** This workflow is comprehensive and consumes significant volume of tokens per complete run through all 4 phases. Be mindful of your Claude Pro usage limits when running this workflow. To reduce token usage during testing, use "approve with no validation" at Checkpoint 3 to skip Phase 4 and/or limit iterations in Discovery and Exploration phases.

**Start the workflow** - You have three options:

   **Option A: Slash Command (Recommended)**

   Explicitly invoke the workflow with:
   ```
   /design-flow [your product idea]
   ```

   Examples:
   ```
   /design-flow habit tracking app
   /design-flow tool to compare apartment listings
   /design-flow meal planning assistant
   ```

   **Option B: Skill (Auto-discovery)**

   Just describe what you want naturally:
   ```
   I want to build a habit tracking app
   Help me create a tool for apartment hunters
   ```

   Claude will automatically recognize your intent and suggest starting the workflow.

   **Option C: Direct Workflow**

   Reference the workflow directly:
   ```
   Follow the design-centered workflow to help me build [your idea]
   ```

3. **Collaborate through the 4 phases:**
   - Claude will guide you through Discovery, Exploration, Design, and Validation
   - Review and provide feedback at each checkpoint
   - Receive a working Next.js prototype with complete documentation

### Example Session

```
You: /design-flow habit tracking tool

Claude: Design-Centered Product Workflow

I'll guide you through a design-first approach to building a product prototype.

We'll go through 4 phases:
1. Discovery - Deep dive into user needs, stakeholders, and market
2. Exploration - Generate and evaluate design concepts
3. Design - Create a working Next.js prototype (documentation generated after approval)
4. Validation - Stakeholder alignment and test scenarios

[... Discovery Phase ...]

CHECKPOINT 1: Discovery Phase Complete

I've created:
- Product Requirements Document (v1)
- 3 User Personas
- User Journey Maps
- Jobs To Be Done Analysis
- Ranked User Needs
- Stakeholder Map
- Competitive Landscape

Does this accurately capture what you want to build?

You: approve

[... Exploration Phase ...]

CHECKPOINT 2: Design Exploration Complete

Generated 4 design concepts:

**SOLUTION 1: Comparison Dashboard** - Score: 8.4/10
[description of concept]

**SOLUTION 2: Gentle Progress Journal** - Score: 8.0/10
[description of concept]

...

Your options:
1. Select concepts - Enter IDs (e.g., "1,3")
2. retry [feedback] - Generate new concepts

You: select 1

[... Design Phase ...]

CHECKPOINT 3: Prototype Generated

Created 11 files in `output/habit-tracker/`

To run:
  cd output/habit-tracker
  npm install
  npm run dev

Open http://localhost:3000

Please test the prototype and review the generated code.

Your options:
1. approve - Move to validation phase
2. approve with no validation - Complete workflow without validation
3. revise [feedback] - Request changes

You: approve

✅ Documentation generated:
   - README.md (setup instructions)
   - DESIGN_PROCESS.md (complete design journey)

[... Validation Phase ...]

CHECKPOINT 4: Validation Phase Complete

Created validation materials:
- STAKEHOLDER_ALIGNMENT.md - How the design addresses each stakeholder's concerns
- TEST_SCENARIOS.md - 4 usability test scenarios with success criteria

Ready for stakeholder presentations and user testing!
```

## 📁 Project Structure

```
.claude/
├── commands/
│   └── design-flow.md              # Slash command for /design-flow
├── skills/
│   └── design-workflow/
│       └── SKILL.md                # Auto-discovery skill definition
├── workflows/
│   └── design-centered.md          # Main workflow orchestration
├── prompts/
│   ├── discovery-phase.md          # Product & UX Researcher role
│   ├── exploration-explorer.md     # Design Explorer role
│   ├── exploration-critique.md     # Design Critic role
│   ├── design-phase.md             # Full-Stack Developer role
│   └── validation-phase.md         # Product & UX Researcher role
└── state/
    └── workflow-*.json             # Conversation state persistence

output/
└── {project-name}/                 # Generated prototypes
    ├── package.json
    ├── app/
    ├── lib/
    └── ...
```

## 🎨 What You Get

### Comprehensive Design Documentation
- Product Requirements Document with stakeholder map
- User research (personas, journeys, Jobs To Be Done)
- Design concept explorations with critiques
- DESIGN_PROCESS.md - Complete design journey (includes all iterations)
- Stakeholder alignment summary (if validation phase completed)
- Usability test scenarios (if validation phase completed)

### Traceability
- **Iteration History** - All user feedback, rejections, and revisions documented
- **Design Decision Audit Trail** - Why concepts were selected or rejected
- **Stakeholder Accountability** - How each stakeholder's concerns were addressed

### Working Prototype
- **Complete Next.js 13+ application** with App Router
- **TypeScript** for type safety
- **Tailwind CSS** for styling
- **Realistic mock data** matching your domain
- **All key screens** from selected concept
- **Ready to run** with `npm install && npm run dev`

## 🔧 Customization

### Modify Role Prompts
Edit files in `.claude/prompts/` to change how each phase works:
- `discovery-phase.md` - Change PRD structure or research methods
- `exploration-explorer.md` - Adjust how concepts are generated
- `exploration-critique.md` - Modify evaluation criteria
- `design-phase.md` - Change prototype requirements
- `validation-phase.md` - Customize validation materials

### Modify Workflow Structure
Edit `.claude/workflows/design-centered.md` to:
- Add new phases
- Change iteration limits
- Adjust quality thresholds
- Modify checkpoint requirements

## 🎯 Use Cases

Perfect for:
- 🚀 **Rapid prototyping** - Get working prototypes in minutes
- 💡 **Product ideation** - Explore multiple approaches before committing
- 📚 **Learning design thinking** - See how structured design process works
- 🎨 **Design education** - Demonstrate UX methodology
- 🤝 **Stakeholder alignment** - Create concrete artifacts for discussion
- 🔬 **Research** - Explore AI-assisted design processes

## 📊 Key Features

- ✅ **No setup required** - Works directly in Claude Code
- ✅ **No API costs** - Included with Claude Pro
- ✅ **Mid-fidelity prototypes** - Working code, not mockups
- ✅ **Full context** - Claude maintains conversation history
- ✅ **Natural interaction** - Conversational checkpoints
- ✅ **State persistence** - Automatically resume interrupted workflows
- ✅ **Multi-workflow support** - Work on multiple product ideas simultaneously
- ✅ **Iterative refinement** - Revise at any checkpoint
- ✅ **Multiple concepts** - Explore solution space before committing
- ✅ **Iteration tracking** - Complete audit trail of design decisions
- ✅ **Flexible workflow** - Skip validation phase to save tokens during testing

## 🔍 How It Works

The workflow uses **role-based prompting** where Claude takes on different roles:

1. **Product & UX Researcher** (Discovery) - Creates PRD, user research, stakeholder map
2. **Design Explorer** (Exploration) - Generates diverse design concepts
3. **Design Critic** (Exploration) - Evaluates concepts on UX criteria
4. **Full-Stack Developer** (Design) - Builds working prototype with DESIGN_PROCESS.md
5. **Product & UX Researcher** (Validation) - Creates resources for stakeholder alignment and user testing

Each role has a detailed prompt in `.claude/prompts/` that Claude reads and follows. The main workflow guide (`.claude/workflows/design-centered.md`) orchestrates the overall flow.

State is saved to `.claude/state/workflow-{timestamp}.json` after each phase, including iteration history for full traceability. The workflow automatically detects and offers to resume any interrupted sessions. You can work on multiple product ideas concurrently, each with independent state.

## 📖 Documentation

- **[CLAUDE.md](CLAUDE.md)** - Complete workflow documentation
  - Detailed phase descriptions
  - State management
  - Customization guide
  - Troubleshooting
  - Best practices

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with and for [Claude Code](https://claude.ai/code) by Anthropic

---

⭐ If you find this workflow useful, please star the repository!
