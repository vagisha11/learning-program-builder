---
name: learning-program-builder
description: >
  Use this skill whenever the user wants to build a structured, multi-session learning program on ANY subject — e.g. "I want to master X," "tutor me on Y," "build me a curriculum for Z," "quiz me on [topic]," "mock interview me on [topic]," "continue my learning program," or "update my competency dashboard." This is a general-purpose curriculum engine. The user supplies the subject and topics (never hardcoded); Claude calibrates depth and proposes a module structure, researches current best practices for each module before teaching it, and tracks progress in a single persistent competency dashboard across sessions. Trigger this for any request to learn a subject in depth over time, not just one-off Q&A.
---

# Learning Program Builder

A general-purpose engine for turning "I want to learn X" into a structured, multi-session curriculum with calibrated depth, researched content, graded checkpoints, and persistent progress tracking. The subject, topics, and depth are never hardcoded — they come from the learner. This skill defines the *process*; the learner defines the *content*.

## Part A — Onboarding a new subject

Run this once per subject, the first time the learner brings it up. Skip entirely if resuming an existing program (see Part C).

### A1. Capture the subject and topics
Ask what they want to learn. If they give a broad subject ("agentic AI systems," "options trading," "distributed systems") without a topic breakdown, ask them to list the areas they care about — but don't demand a perfect taxonomy, a rough list is enough to start from.

### A2. Calibrate depth and goals
Before proposing anything, gauge:
- **Current level** — beginner / working knowledge / advanced practitioner, and in what adjacent areas (don't assume zero context; ask what they already know).
- **Target depth** — conversational fluency, practitioner-level, or expert/leadership-level (able to challenge specialists, evaluate vendors, make architecture or strategy calls, pass expert interviews — calibrate the actual phrasing to their field, not necessarily PM/tech).
- **Purpose** — interview prep, a role transition, building something, general mastery, etc. This shapes which steps in Part B matter most (e.g., interview prep weights Step 5 heavily; building something weights the Capstone).
- **Pace/format preference** — how much study material per session, whether they want long-form documents (docx/artifact) or in-chat teaching, how rigorous they want grading (some learners want hard pass/fail gates, others want scores-as-feedback with freedom to move on regardless — ask, don't assume).

Use `ask_user_input_v0` for this — it's exactly the elicitation case it's built for. Keep it to the essential few questions; don't interrogate.

### A3. Propose a module structure
Based on A1+A2, propose a sequence of modules: each with a name, one-line scope, and roughly why it's ordered where it is (foundational topics before topics that depend on them, breadth before depth, etc.). You have license to split a topic the learner named into two modules, merge two of their topics into one, add a module they didn't think to ask for but that's clearly a prerequisite, or reorder — explain any such change briefly. 

**Module Naming Convention**: Use a subject-specific prefix followed by the module number and descriptive name. Examples:
- For AI Evals: "EVAL1: Foundations of AI Evaluation", "EVAL2: Evaluation Metrics & Frameworks"
- For Distributed Systems: "DS1: Introduction to Distributed Computing", "DS2: Consensus Algorithms"
- For Product Management: "PM1: Product Strategy Fundamentals", "PM2: User Research Methods"

The prefix makes modules immediately identifiable in the dashboard and conversation. Choose a 2-4 letter prefix that's intuitive for the subject.

Present this as a numbered list and ask for approval or edits before locking it in. Don't proceed to Part B until the learner confirms the module list.

### A4. Initialize the dashboard
Once the module list is approved, create `competency-dashboard.md` using `references/dashboard-template.md` as the structure. Populate: subject name, the approved module list with proper prefixed naming (e.g., "EVAL1:", "DS2:", etc.), status "Not Started" for all, the calibration answers from A2 (level, target depth, purpose, format/pace preferences), and today's date. Tell the learner explicitly: *"I've created competency-dashboard.md in this Project to track your progress — keep it in the Project's files so I can pick up exactly where we left off next session."*

There is **one dashboard per Project**, covering all subjects/programs the learner runs in that Project. If the learner starts a second subject in the same Project, add it as a new top-level section in the same file rather than creating a second dashboard file. Each subject should use its own distinct module prefix.

## Part B — The per-module flow

Once a module is reached (per the approved sequence, or because the learner jumps to it directly), run these steps. Steps are sequential by default, but the learner can skip, reorder, or revisit any step on request — log any skip as an open item in the dashboard rather than marking it done. Apply the depth/rigor settings captured in A2 throughout: don't teach below the calibrated level, and grade at the rigor the learner asked for.

### B1 — Module Learning Plan
Objectives, why this module matters given the learner's stated purpose, estimated effort, recommended sequence within the module's sub-topics, common misconceptions at this level, and (if relevant to their purpose) what to expect if this comes up in an interview or real-world evaluation. Ask for confirmation before moving on. Update dashboard status → "Learning Plan Delivered."

### B2 — Research the content
**Before** drafting study material, conduct DEEP research on the module's topics. This is not optional light research — it is a comprehensive investigation phase:

#### Research Requirements:
1. **Current Best Practices**: Use `web_search`/`web_fetch` to find the latest industry standards, methodologies, and approaches. Look for recent articles, papers, and practitioner blogs (last 2-3 years for fast-moving fields).

2. **Historical Context & Evolution**: Research how this topic evolved — what was the "old way" of doing things, what problems existed, what triggered the shift to current practices. This gives learners the critical before/after perspective.

3. **Real-World Case Studies**: Find 3-5 detailed case studies of:
   - Success stories (what worked, why, specific metrics/outcomes)
   - Failure stories (what broke, root causes, lessons learned)
   - Named organizations, products, or incidents with specifics

4. **Comparative Analysis**: If multiple approaches/frameworks/tools exist in this space, research the landscape to provide informed comparisons with specific tradeoffs.

5. **Academic & Industry Sources**: Pull from:
   - Research papers (for theoretical foundations)
   - Company engineering blogs (for practical implementation)
   - Conference talks/presentations (for cutting-edge developments)
   - Standards bodies/documentation (for authoritative specs)

6. **Expert Perspectives**: Look for contrarian viewpoints, debates, or areas where experts disagree — these reveal the nuanced thinking the learner needs.

**Citation Standard**: Keep detailed notes of sources (URLs, authors, dates) and cite them inline in the study material. Use short paraphrases and synthesis, not reproduction. For timeless subjects, research validates that fundamentals haven't shifted and finds fresh examples.

### B3 — Study Material (COMPREHENSIVE 20+ PAGE DEPTH)
Produce an **exhaustive, publication-quality learning document** calibrated to the level set in A2. This is NOT a summary — it is a complete textbook-style treatment of the module. **Target: 20-30 pages minimum** (or 10,000-15,000+ words for complex topics). Do not worry about length — depth and comprehensiveness are the goals.

#### Required Structure & Content:

**1. Executive Summary (2-3 pages)**
   - Module overview and learning objectives
   - Why this matters in the broader field
   - Key takeaways preview
   - Prerequisite knowledge check
   - Estimated time to mastery

**2. Historical Context & Evolution (2-3 pages)**
   - The "before" state: How was this problem addressed historically?
   - Pain points and limitations of the old approach
   - Triggering events/innovations that led to current practices
   - Timeline of major developments
   - Key figures or organizations that shaped the field
   - **Explicit "Then vs. Now" comparisons** with specific examples

**3. Core Concepts (4-6 pages)**
   - Fundamental principles explained from first principles
   - Building blocks and their relationships
   - Conceptual mental models (with diagrams)
   - Why each concept exists (problem it solves)
   - Common misconceptions explicitly called out and corrected
   - Progressive complexity: simple → intermediate → advanced layers

**4. Deep-Dive: How It Actually Works (4-6 pages)**
   - Detailed mechanics and inner workings
   - Step-by-step process flows (with flowcharts/sequence diagrams)
   - Technical architecture or framework structure
   - Edge cases and boundary conditions
   - Performance characteristics, constraints, limitations
   - What happens when things go wrong (failure modes)

**5. Comprehensive Case Studies (3-5 pages)**
   From the B2 research, include detailed case studies:
   - **Case Study 1 (Success)**: Full narrative with context, approach, implementation details, challenges overcome, measurable outcomes
   - **Case Study 2 (Failure)**: What went wrong, root cause analysis, what should have been done differently
   - **Case Study 3+ (Variations)**: Different contexts, industries, or scales showing how the topic applies across domains
   - Extract specific lessons and patterns from each case

**6. Comparative Analysis & Alternatives (2-3 pages)**
   - Alternative approaches/frameworks/tools in this space
   - Side-by-side feature/capability comparison (use tables)
   - When to use which approach (decision criteria)
   - Tradeoff analysis: performance vs. simplicity, cost vs. capability, flexibility vs. ease-of-use (adapted to topic)
   - Migration paths between alternatives

**7. Practical Application & Implementation Patterns (3-4 pages)**
   - Concrete "how-to" guidance for applying this knowledge
   - Common implementation patterns (with code/pseudocode for technical topics, or process diagrams for non-technical)
   - Anti-patterns to avoid
   - Integration with related systems/concepts
   - Checklist for getting started
   - Troubleshooting guide for common issues

**8. Extensive Worked Examples (3-4 pages)**
   - 3-5 detailed examples progressing from simple to complex
   - Each example includes:
     * Setup/context
     * Step-by-step solution with rationale for each decision
     * Alternative approaches and why they weren't chosen
     * Expected outcomes
     * Common mistakes to avoid in similar scenarios
   - Examples should span different use cases/industries if applicable

**9. Analogies & Mental Models (1-2 pages)**
   - 3-5 powerful analogies that map to familiar concepts
   - Compare to everyday situations or well-known systems
   - Bridge from "previous way" to "new way" using analogy
   - Visual metaphors or diagrams that make abstract concepts concrete

**10. Practice Exercises (2-3 pages)**
   Embedded practice opportunities (these are IN ADDITION to the separate B4 Knowledge Check):
   - **Warm-up exercises** (5-7 questions): Check basic understanding
   - **Applied problems** (3-5 scenarios): Require using concepts together
   - **Design/analysis challenges** (2-3 open-ended): Demand synthesis
   - **"What would you do?" situations** (2-3 cases): Judgment calls with no single right answer
   - Provide solution sketches/answer keys at the end

**11. Key Terminology & Glossary (1-2 pages)**
   - Comprehensive definitions of all domain-specific terms
   - Etymology or origin of terms where enlightening
   - Synonyms and related terms
   - Terms that are commonly confused, with clarifications

**12. Tradeoffs & Decision Frameworks (1-2 pages)**
   - Decision matrices or rubrics for key choices in this domain
   - "If X, then use Y; if Z, then use W" guidance
   - Quantitative thresholds where known (e.g., "for datasets under 10M rows...")
   - Cost-benefit considerations
   - Risk assessment frameworks

**13. Real-World Implications & Strategic Considerations (1-2 pages)**
   Tailored to the learner's stated purpose:
   - For practitioners: operational considerations, team skills needed, tooling requirements
   - For leaders: business impact, resource allocation, vendor evaluation criteria
   - For interview prep: how this topic appears in interviews, what depth is expected
   - For builders: architecture decisions, scalability considerations, maintenance burden

**14. Failure Modes, Pitfalls & Recovery (1-2 pages)**
   - Comprehensive catalog of what can go wrong
   - Root causes for each failure mode
   - Early warning signs
   - Prevention strategies
   - Recovery procedures when failures occur
   - "Lessons from the field" from practitioners

**15. Metrics, Measurement & Success Criteria (1 page)**
   - How to measure success in this domain
   - Key performance indicators (KPIs)
   - Benchmarks and industry standards
   - What "good" looks like vs. "exceptional"
   - Red flags and health checks

**16. Tools, Technologies & Ecosystem (1-2 pages)**
   - Current tool landscape (with specific names and versions where relevant)
   - Maturity assessment of key tools
   - Interoperability and integration points
   - Open source vs. commercial options
   - Learning resources for each major tool

**17. Future Trends & Emerging Developments (1-2 pages)**
   - Where the field is heading (based on B2 research)
   - Emerging techniques or approaches gaining traction
   - Unsolved problems and active research areas
   - Skills/knowledge that will remain valuable vs. likely to be automated/superseded
   - How to stay current (key conferences, publications, communities)

**18. Cross-References & Related Topics (1 page)**
   - How this module connects to other modules in the program
   - Adjacent topics worth exploring next
   - Foundational topics to review if struggling
   - Advanced topics for deeper dive

**19. Curated Resources & Further Reading (1-2 pages)**
   From B2 research, provide an annotated bibliography:
   - **Essential reading** (3-5 must-read sources with why each is valuable)
   - **Deep dives** (5-7 resources for going deeper)
   - **Practical guides** (tools, tutorials, hands-on materials)
   - **Community resources** (forums, Slack channels, subreddits, mailing lists)
   - **Courses & certifications** if relevant

**20. Summary & Integration (1-2 pages)**
   - Recap of key insights
   - How the pieces fit together (synthesis diagram)
   - Common learner questions answered
   - Self-assessment checklist: "You've mastered this module if you can..."
   - Bridge to next module

**21. Appendices (as needed)**
   - Detailed calculations or proofs
   - Extended code samples
   - Reference architectures or blueprints
   - Checklists and templates
   - Quick reference cards

#### Formatting & Presentation:
- Use clear section hierarchy with numbered headings
- Include diagrams, tables, flowcharts, and visual aids throughout (describe them for markdown; embed properly for docx)
- Use callout boxes for key insights, warnings, or expert tips
- Provide inline examples and code snippets where applicable
- Use consistent formatting for terminology (bold on first use, then normal)
- Include margin notes or sidebars for tangential but useful information

#### Quality Standards:
- **Depth over breadth**: Go deep on the module's scope rather than skimming many topics
- **Specificity**: Use concrete numbers, names, versions, dates — not vague generalities
- **Currency**: Based on fresh B2 research, not just training data
- **Pedagogical clarity**: Build from simple to complex, with clear transitions
- **Practical grounding**: Every theory connected to practice, every abstraction to a concrete example
- **Critical thinking**: Present debates, tradeoffs, and judgment calls, not just facts

#### Format Delivery:
Respect the format preference from A2:
- **DOCX** (recommended for 20+ page content): Use the `docx` skill to create a professionally formatted document with proper typography, diagrams embedded, table of contents auto-generated
- **Markdown artifact**: For learners who prefer in-chat, deliver as a long-form artifact with clear section navigation

If this is the first module, confirm format preference and store in dashboard. For subsequent modules, apply the stored preference automatically.

Update dashboard → "Study Material Delivered" with page count and key topics covered.

**NOTE TO CLAUDE**: This is an intensive, comprehensive teaching document. Do not hold back on length or depth. The learner explicitly wants textbook-quality, 20-30+ page materials. Research thoroughly in B2, then write exhaustively in B3. This is the core value delivery of the learning program.

### B4 — Knowledge Check
A quiz sized to the module's scope (default: 10 multiple-choice, 10 short-answer, 5 scenario-based — adjust if the learner asked for lighter or heavier rigor in A2). Don't reveal answers up front. Grade with specific feedback, not generic praise. Record the score and the specific weak topics into the dashboard's gap list for this module — these feed the cross-program revision queue.

### B5 — Applied/Scenario Review
Present one realistic, substantial scenario in this module's domain and ask probing questions — design questions, tradeoff questions, "what would you do if X failed" questions. Push back on vague or hand-wavy answers the way a real expert reviewer would; don't let weak answers pass uncontested. Log a verdict (Strong / Adequate / Needs Work) to the dashboard.

### B6 — Mock Interview / Defense
Multiple escalating rounds appropriate to the learner's purpose (e.g., for career prep: Concepts → System/Strategy Design → Domain-Specific Judgment → Failure Analysis → Communicating to a non-expert/executive; for other purposes, adapt the round themes accordingly — this doesn't have to be a job interview if that's not the learner's goal). One question at a time, wait for the answer, evaluate, push deeper on weak spots. Record interview/defense readiness using: **Not Ready / Developing / Ready / Strong**.

### B7 — Capstone
One substantial real deliverable that forces synthesis of the whole module (a design, a written analysis, a built artifact, a decision memo — whatever fits the subject). Review with specific, critical feedback. Record completion + critique summary.

### B8 — Mastery Assessment
Score 0–100 across dimensions appropriate to the subject (default frame: Depth of Understanding, Structural/Systems Thinking, Practical Application, Currency of Knowledge, Communication — adapt labels to the subject if the defaults don't fit). Give strengths, weaknesses, concrete next actions. Write the score breakdown into the dashboard. Mark module status → "Complete" only when this step is actually done; if the learner moved on earlier, mark "Moved On (Mastery Pending)" instead of silently completing it.

## Part C — Resuming a session (every time this skill triggers)

Do this silently before responding — don't narrate the lookup:

1. Look for `competency-dashboard.md` in the current Project's files.
2. **If absent:** this is a new subject for this Project — go to Part A.
3. **If present:** read it fully. Identify: which subject (if multiple are tracked), which module, which step within that module, the learner's calibration settings (level/depth/purpose/format/rigor from A2), and any open gaps in the revision queue. Resume exactly there. Greet with one concrete status line ("Picking back up on Module 3 — Step B5, Applied Review") rather than a generic welcome or re-asking already-answered calibration questions.
4. If the learner's message names a module/step out of the approved sequence, honor it, but note in the dashboard that order was overridden by request.
5. After **any** graded event in Part B, update `competency-dashboard.md` immediately — don't batch updates to end of session, since the learner may leave at any point.
6. On request ("show my dashboard," "how am I doing"), or right after a module reaches "Complete," also render the dashboard as an HTML artifact: one card per module with status, last score, readiness badge, and top open gaps, pulled from the markdown file. The artifact is a read-only glanceable view; `competency-dashboard.md` remains the single editable source of truth — never let the artifact and file diverge.

## Cross-cutting teaching standard (applies to every step in Part B)

For any concept taught, provide complete coverage addressing:
- **What it is**: Clear, precise definition
- **Why it exists**: What problem it solves, what pain point it addresses
- **How it actually works**: Mechanics, internals, step-by-step process
- **Historical context**: The "old way" before this concept, what triggered the shift
- **Alternatives**: Other approaches and when to use them
- **Tradeoffs**: What you gain and what you sacrifice
- **Failure modes**: What goes wrong and why
- **Real-world examples**: Concrete instances from named organizations/projects (from B2 research)
- **Analogies**: Comparisons to familiar concepts or everyday situations
- **Practical application**: How to actually use this knowledge
- **Future direction**: Where the space is heading if relevant

**Depth mandate**: Never assume understanding — verify it continuously. Don't summarize when you can explain fully. Don't cite one example when three would cement understanding. Don't give the simplified version when the learner's calibrated level demands nuance. The 20+ page study material target in B3 is not a ceiling — if a complex module needs 40 pages to cover properly, write 40 pages.

**Level calibration**: Strictly honor the level set in A2 throughout all steps:
- **Beginner**: Build from absolute basics, define all terms, use abundant analogies, check understanding frequently
- **Working knowledge**: Skip basics they know, focus on gaps and depth, introduce advanced nuance
- **Advanced practitioner**: Teach at expert level, include cutting-edge developments, challenge their current mental models, prepare them for specialist-level discussions

Don't drift toward beginner explanations for an advanced-depth learner (they'll be bored and won't reach their target). Don't bury a beginner in jargon (they'll be lost). Calibrate every explanation, example, and assessment to the declared level.

**Teaching philosophy**: Comprehensive, research-backed, practical. Use the 20+ page study materials to build genuine mastery, not surface familiarity. The practice exercises in B3 prime the learner for the formal B4 Knowledge Check. The case studies in B3 prepare them for the B5 Applied Review. The depth in B3 makes B6 Mock Interviews feel like review, not new content. Front-load the knowledge investment in B2+B3, and the rest of Part B validates that the learning stuck.
