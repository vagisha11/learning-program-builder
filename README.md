# Learning Program Builder Skill

A general-purpose Claude Code skill that transforms "I want to learn X" into a structured, multi-session curriculum with calibrated depth, researched content, graded checkpoints, and persistent progress tracking.

## Overview

This skill provides a complete framework for building personalized learning programs on **any subject**. It's not domain-specific—you supply the subject and topics, and the skill creates a rigorous, adaptive curriculum with continuous assessment and progress tracking across sessions.

## What It Does

- **Calibrated Curriculum Design**: Assesses your current level, target depth, and learning goals to create a custom module sequence
- **Research-Backed Content**: Uses web search to incorporate current best practices, real-world examples, and up-to-date terminology
- **Multi-Modal Assessment**: Knowledge checks, scenario reviews, mock interviews/defenses, and capstone projects
- **Persistent Progress Tracking**: Single competency dashboard per project that tracks scores, gaps, and readiness across all modules
- **Flexible Pacing**: Supports strict gated progression or flexible learning with scores-as-feedback

## Repository Structure

```
.
├── README.md                              # This file
├── learning-program-builder.skill         # Packaged skill (zip archive)
├── SKILL.md                               # Skill instructions (extracted reference)
└── competency-dashboard-template.md       # Dashboard template (extracted reference)
```

The `.skill` file is a zip archive containing:
- `learning-program-builder/SKILL.md` - Complete skill instructions for Claude
- `learning-program-builder/references/dashboard-template.md` - Template for progress tracking

## Installation

1. Install the skill in Claude Code:
   ```bash
   # From this directory
   claude skill add learning-program-builder.skill
   ```

2. Verify installation:
   ```bash
   claude skill list
   ```

## Usage

### Starting a New Learning Program

Simply tell Claude what you want to learn:

```
"I want to master distributed systems"
"Build me a curriculum for options trading"
"Tutor me on agentic AI systems"
```

Claude will guide you through:
1. **Subject & Topics**: Define what you want to learn
2. **Calibration**: Set your current level, target depth, purpose, and pace preferences
3. **Module Structure**: Review and approve the proposed learning sequence
4. **Dashboard Initialization**: Create a `competency-dashboard.md` file in your project

### Continuing an Existing Program

When you return to learning, just say:

```
"Continue my learning program"
"Let's pick up where we left off"
"Show my progress dashboard"
```

Claude automatically reads your `competency-dashboard.md` and resumes exactly where you stopped.

### Per-Module Learning Flow

Each module follows a structured 8-step process:

1. **Learning Plan** - Objectives, effort estimate, sequence, common misconceptions
2. **Deep Research** - Comprehensive investigation of current best practices, historical context, case studies, expert perspectives, and comparative analysis via web search
3. **Study Material (20-30+ pages)** - Exhaustive, textbook-quality learning document with detailed case studies, worked examples, practice exercises, analogies, and comprehensive coverage of the topic
4. **Knowledge Check** - Multiple-choice, short-answer, and scenario-based quiz
5. **Applied/Scenario Review** - Realistic scenario with probing questions
6. **Mock Interview/Defense** - Escalating rounds testing concept mastery
7. **Capstone** - Substantial deliverable synthesizing the module
8. **Mastery Assessment** - 0-100 scoring across multiple dimensions

You can skip, reorder, or revisit steps on request—everything is logged in the dashboard.

#### What Makes the Study Material Different

The B3 Study Material step is designed to be **exceptionally comprehensive** (20-30+ pages or 10,000-15,000+ words):

- **Deep Historical Context**: "Before and after" comparisons showing how the field evolved
- **Extensive Case Studies**: 3-5 detailed real-world examples from named organizations with success and failure narratives
- **Comprehensive Examples**: Multiple worked examples progressing from simple to complex
- **Built-in Practice**: Embedded exercises with solution keys (in addition to the separate B4 quiz)
- **Rich Analogies**: Comparisons to familiar concepts and everyday situations
- **"Then vs. Now" Analysis**: Explicit comparisons between old and new ways of working
- **Complete Coverage**: 21-section structure covering everything from fundamentals to future trends
- **Research-Backed**: Based on fresh web research, not just training data

## Key Features

### Adaptive Depth
- **Beginner**: Foundational concepts with clear explanations
- **Working Knowledge**: Practical application and real-world patterns
- **Advanced Practitioner**: Expert-level tradeoffs, architecture decisions, strategic judgment

### Purpose-Driven
- Interview prep (heavy emphasis on mock interviews)
- Role transition (focus on practical application)
- Building something (weighted toward capstone projects)
- General mastery (balanced across all assessment types)

### Rigorous Assessment
- Specific feedback, not generic praise
- Push back on vague or weak answers
- Critical capstone reviews
- Multi-dimensional mastery scoring

### Progress Transparency
Every graded event is immediately logged to `competency-dashboard.md`:
- Module status and completion
- Knowledge check scores
- Applied review verdicts
- Interview/defense readiness
- Mastery scores and dimension breakdown
- Identified gaps for revision

## Dashboard Structure

The `competency-dashboard.md` serves as the single source of truth for all learning programs in a project:

- **Calibration settings** - Level, depth, purpose, format preferences, rigor
- **Module status overview** - Quick view of progress across all modules
- **Module detail** - Scores, verdicts, mastery breakdowns per module
- **Revision queue** - Cross-module weak topics for targeted review
- **Session log** - Historical record of what was covered when

Multiple subjects can be tracked in the same dashboard file (one section per subject).

## Example Workflow

```
You: "I want to master prompt engineering for AI agents"

Claude: [Asks calibration questions about your level, goals, pace]

You: [Answers: Working knowledge, target expert level, building production systems, docx format, strict grading]

Claude: [Proposes 5-module structure]
   Module 1: Foundations of Prompt Engineering
   Module 2: Agent Architecture & Tool Use
   Module 3: Multi-Agent Orchestration
   Module 4: Evaluation & Iteration
   Module 5: Production Patterns & Safety

You: "Looks good, let's start"

Claude: [Creates competency-dashboard.md, delivers Module 1 Learning Plan]

[Work through Module 1 steps B1-B8]

[Next session]

You: "Continue my learning program"

Claude: "Picking back up on Module 2 — Step B3, Study Material"
```

## File Formats

### Study Material
- **Docx**: Full-featured documents via the `docx` skill (recommended for long-form content)
- **Markdown**: In-chat artifacts for quick reference

### Dashboard
- Always markdown (`competency-dashboard.md`)
- HTML artifact available on request for visual overview
- Markdown file is the single editable source of truth

## Best Practices

1. **Keep the dashboard in your project files** - Claude needs persistent access across sessions
2. **Be honest during calibration** - Accurate level assessment ensures appropriate challenge
3. **Don't skip assessment steps** - They identify gaps and ensure mastery
4. **Review your dashboard periodically** - Use "show my progress dashboard" to track growth
5. **Update your goals if they change** - Tell Claude to adjust depth or focus mid-program

## Design Philosophy

### Subject-Agnostic
The skill defines the **process** (how to learn), not the **content** (what to learn). It works equally well for technical subjects, business domains, creative skills, or academic topics.

### Research-First & Depth-Oriented
Claude conducts deep research before every module (B2), investigating:
- Current best practices and methodologies
- Historical evolution and "before/after" context
- Real-world case studies (successes and failures)
- Expert perspectives and comparative analysis
- Academic sources, industry blogs, and standards

This research feeds into **20-30+ page comprehensive study materials** (B3) that include:
- Detailed explanations built from first principles
- Multiple case studies with specific organizations and outcomes
- Extensive worked examples and practice exercises
- Analogies bridging "old ways" to "new ways"
- Complete 21-section coverage from history to future trends

The depth is intentional—you get textbook-quality materials that build genuine mastery, not surface familiarity.

### Verification Over Assumption
Never assumes understanding. Every step includes specific checks: quizzes grade knowledge, scenarios test application, interviews probe depth, capstones demand synthesis.

### Transparency
All calibration settings, module sequences, scores, gaps, and next steps are visible in the dashboard. No black boxes.

## Troubleshooting

### Dashboard Not Found
- Ensure `competency-dashboard.md` is in your project directory
- If starting fresh, Claude will create it automatically during onboarding

### Wrong Module/Step
- Tell Claude directly: "Go to Module 3" or "Skip to capstone"
- All overrides are logged in the dashboard

### Format Preference Changed
- Just ask: "Use markdown instead of docx going forward"
- Claude updates the dashboard and applies it to future modules

### Need to Restart a Module
- Request: "Let's redo Module 2 assessment"
- Previous scores remain in history

## Version History

**Current Version**: 2.0 (as of June 2026)

### Version 2.0 Changes
- **Enhanced Research Phase (B2)**: Added comprehensive research requirements covering historical context, case studies, comparative analysis, and expert perspectives
- **Expanded Study Material (B3)**: Upgraded from summary-style to 20-30+ page comprehensive textbook-quality documents
- **21-Section Structure**: Detailed content template including historical evolution, extensive case studies, worked examples, practice exercises, analogies, and "then vs. now" comparisons
- **Embedded Practice**: Added practice exercises within study materials (in addition to separate knowledge checks)
- **Depth Mandate**: Removed length concerns—prioritize comprehensive coverage over brevity

### Version 1.0 (Original)
- Basic 8-step learning flow
- Standard study material with core sections
- Knowledge checks and assessments
- Dashboard tracking

## Support & Feedback

This is a Claude Code skill. For questions or issues:
- Check your `competency-dashboard.md` for current state
- Ask Claude directly about any step in the learning flow
- Review this README for usage patterns

## License

This skill is part of the Claude Code skill ecosystem. Refer to Claude Code documentation for licensing terms.

---

**Ready to learn?** Just tell Claude what subject you want to master, and the skill handles the rest.
