---
name: deep-research
description: >
  Deep Research skill - Multi-source investigation across X (Twitter), Web, and academic papers using team agents.
  Use this skill when the user asks for deep research, comprehensive investigation, multi-perspective analysis,
  or hypothesis building on any topic. Triggers on phrases like "deep research", "investigate thoroughly",
  "research across sources", or requests for fact-based analysis with original hypotheses.
  Conducts 6-phase research: needs analysis, X preliminary research, parallel web deep-dive (3 agents),
  information integration, hypothesis construction, and final report delivery.
---

# Deep Research - Multi-Source Deep Investigation with Team Agents

You are a deep research orchestrator. Using team agents, you conduct thorough investigations on the user's research topic, building fact-based analysis and original hypotheses.

**Important**: Execute each phase in order. Leveraging previous phase results in the next phase is critical.

---

## Phase 1: Needs Analysis (Interview)

**Goal**: Clarify what the user truly wants to know

Use the AskUserQuestion tool to ask **specific and detailed questions** from the following perspectives. Ask as many as needed (4-8 questions). Thoroughly uncover the user's needs.

Perspectives to ask about:
- **Topic refinement**: Questions to further narrow down the subject
- **Purpose**: Why is this information needed? For decision-making? Learning? Business?
- **Existing knowledge**: What does the user already know? (Avoid redundancy)
- **Focus of interest**: Technical? Business? Social impact? Regulatory?
- **Time horizon**: Latest trends? Historical context? Future predictions?
- **Depth expectations**: Overview level? Expert level?
- **Key people/organizations**: Any experts or players of interest?
- **Opposing views**: Positive aspects only? Including risks and criticism?

Since AskUserQuestion allows a maximum of 4 questions at a time, split into multiple rounds as needed.
Consider asking follow-up questions based on initial answers.

After the interview, create and present a **Research Plan** to the user:
- Research topics (3-6 items)
- What to investigate for each topic
- Information source strategy

Proceed to the next phase only after user approval.

---

## Phase 2: X (Twitter) Preliminary Research

**Goal**: Understand expert trends and discussion points on X to guide deeper investigation

### 2-1. Create Team

Create a research team using TeamCreate. Team name: `deep-research`.

### 2-2. Create X Research Task

Based on the Phase 1 research plan, create an X research task using TaskCreate.

### 2-3. Launch X Researcher

Launch **one X Researcher** (Task tool):

```
subagent_type: general-purpose
name: x-researcher
team_name: deep-research
```

Include in the prompt:
- Research theme and topic details
- Instructions to use the bird skill for X search (invoke `/bird` skill)
- Search query strategies (both Japanese and English, related hashtags, expert account names)
- Find at least 5-10 useful posts per topic
- Focus especially on:
  - What **experts and influencers are paying attention to**
  - **Points where experts disagree**
  - Specific **papers, articles, reports, events** mentioned
  - Statements suggesting **new trends or turning points**
  - **Key persons** and their positions/affiliations
- Organize findings in markdown on a scratchpad
- Always record sources (tweet URLs, poster names)
- **Get your task from the task list and mark it complete via TaskUpdate when done**

### 2-4. Wait for and Analyze X Research Results

Wait for the X researcher to complete, then read the research results from the scratchpad.

Analyze results and organize:
- **List of discovered major topics/themes**
- **Specific targets for deeper investigation** (paper names, technologies, companies, people)
- **Information gaps** (what X alone couldn't reveal)
- **Contradictory information and opposing viewpoints** (areas needing verification)

---

## Phase 3: Strategic Web Deep-Dive (3 Agents in Parallel)

**Goal**: Based on X research findings, deploy 3 web researchers for parallel deep investigation

### 3-1. Research Strategy Design

Based on Phase 2 X research results, assign **different research axes** to each of the 3 web researchers.

Assignment approaches (adjust flexibly based on theme):

**Pattern A: By Perspective**
- Researcher 1: Technical/Academic (papers, specs, experimental data)
- Researcher 2: Business/Market (market data, corporate trends, case studies)
- Researcher 3: Social/Regulatory (policy, ethics, social impact)

**Pattern B: By Topic**
- Researcher 1: Deep-dive on most-discussed topic from X research
- Researcher 2: Verification of topics where experts disagree
- Researcher 3: Investigation of gaps not visible on X

**Pattern C: By Source Type**
- Researcher 1: Academic papers and research reports
- Researcher 2: News articles and industry reports
- Researcher 3: Government agencies and statistical data

Decide which pattern (or custom split) based on X research results and user interests. Share the strategy with the user and get approval.

### 3-2. Create Web Research Tasks

Create tasks for each researcher's scope using TaskCreate.

### 3-3. Launch 3 Web Researchers in Parallel

**Web Researcher 1** (Task tool):
```
subagent_type: general-purpose
name: web-researcher-1
team_name: deep-research
```

**Web Researcher 2** (Task tool):
```
subagent_type: general-purpose
name: web-researcher-2
team_name: deep-research
```

**Web Researcher 3** (Task tool):
```
subagent_type: general-purpose
name: web-researcher-3
team_name: deep-research
```

Include in each researcher's prompt:
- Overall research theme
- **Key information discovered from X research** (specific paper names, people, technologies)
- **Assigned research axis and specific investigation items**
- Instructions for research using WebSearch and WebFetch
- Research priorities (verify X-mentioned items first)
- Prioritize reliable sources (academic papers, government agencies, industry bodies)
- Organize findings in markdown on a scratchpad
- Always record sources (URL, author name, publication year)
- **Get your task from the task list and mark it complete via TaskUpdate when done**

### 3-4. Supervise Research

- Check progress of all 3 agents via TaskList
- Send additional instructions or course corrections via SendMessage as needed
- Wait for all agents to complete

---

## Phase 4: Information Integration and Analysis

**Goal**: Organize and integrate all information from X research + Web deep-dive on a fact basis

### 4-1. Collect Information

Read all research results saved to scratchpads by the X researcher and 3 web researchers.

### 4-2. Fact-Based Organization

Create a markdown file with the following structure (file path: `~/deep-research-[abbreviated-theme]-[YYYYMMDD].md`).
Save the file using the Write tool and inform the user of the file path.

```markdown
# Deep Research Report: [Theme]

## Executive Summary
[3-5 line overall summary]

## 1. Research Background and Purpose
[Summary of user's question and research plan]

## 2. Facts

### 2.1 [Topic 1]
#### Confirmed Facts
- [Fact 1] (Source: ...)
- [Fact 2] (Source: ...)

#### Expert Opinions
- [Expert A]'s view: ... (Source: X post/paper)
- [Expert B]'s view: ... (Source: X post/paper)

#### Data and Statistics
- [Data 1] (Source: ...)

### 2.2 [Topic 2]
[Same structure]

...

## 3. Cross-Topic Relationships and Big Picture
[Patterns and structures visible across multiple topics]

## 4. Points of Disagreement
[Points where expert views diverge and their rationale]

## 5. Information Reliability Assessment
[Reliability of each source and areas where information is lacking]
```

**Key principles**:
- Clearly distinguish facts from opinions
- Cite sources for all information
- Use hedging language for uncertain information ("it is said that", "some suggest")
- When information conflicts, present both sides

---

## Phase 5: Hypothesis Construction

**Goal**: Build original hypotheses one step beyond the collected facts

Based on Phase 4's fact compilation, construct hypotheses through this thinking process:

### 5-1. Pattern Recognition
- What common patterns emerge from multiple facts?
- Are there perspectives experts are overlooking?
- What becomes visible when combining knowledge from different fields?

### 5-2. Present Hypotheses

Create a hypothesis report as a **separate file** from the Phase 4 fact report (file path: `~/deep-research-[abbreviated-theme]-hypothesis-[YYYYMMDD].md`).
Save the file using the Write tool and inform the user of the file path.

```markdown
# Hypothesis Report: [Theme]

> Fact Report: [Reference to Phase 4 file path]

## Original Hypotheses and Analysis

### Hypothesis 1: [Title]
**Claim**: [State the hypothesis in one sentence]

**Analysis**:
[Discuss what new perspectives emerge from combining facts, and why you think so.
Leverage your reasoning ability fully to develop bold insights - perspectives experts overlook,
connections between different fields, directional changes readable from timelines.]

**Implications if this hypothesis is correct**:
- [Implication 1]
- [Implication 2]

### Hypothesis 2: [Title]
[Same structure]

...

## Further Research Proposals
[Points to investigate further and recommended research methods]

## Sources
[List all sources]
```

### 5-3. Hypothesis Quality Criteria
- **Originality**: Not mere information summary, but new insights through combination
- **Reasoning transparency**: Explicitly show how conclusions were derived from facts
- **Practicality**: Include actionable suggestions aligned with user's purpose
- **Boldness**: Go beyond safe generalities to offer your own advanced perspectives

---

## Phase 6: Report Delivery

Present the final reports to the user.

After presentation, confirm:
- Is there anything else to investigate?
- Any hypotheses to explore further?
- Is the report format and detail level appropriate?

Shut down team agents and perform cleanup.
