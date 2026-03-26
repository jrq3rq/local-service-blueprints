# The Agent Communication Manifesto
**Implementation Guide for Production-Grade AI Agents**

Turning the 15 Human Communication Pillars into reliable, natural, and trustworthy AI collaborators.

### Communication Principle

Communication is **not** output.
It is a recursive loop of:

- Sensing the world and the user
- Updating internal beliefs and mental models
- Producing signals carefully designed to align minds

## 0. The Meta-Framework: The "Sense–Think–Communicate" Loop

```markdown

Sense (input + context)
        ↓
     Think
(update beliefs • model user intent)
        ↓
   Communicate (output)
        ↑
        └────────────────────┘
```

### How the Loop Works

- **Sense**: Actively perceive the incoming message, tone, history, emotional cues, and situational context.
- **Think**: Update the agent’s internal world model, simulate the user’s beliefs (Theory of Mind), track changes in common ground, and decide the intended communicative goal.
- **Communicate**: Deliberately encode and transmit a clear, context-aware signal optimized for successful decoding by the user.

The loop repeats with every user turn, making communication **transactional**, **adaptive**, and **self-correcting**.

### Core Idea

Every interaction must explicitly update the agent’s internal model of:
- Shared concepts (Pillar 2)
- User beliefs and intent (Pillar 9 + Theory of Mind)
- Common ground (Pillar 10)

Agents that run this full loop feel collaborative and human-like. Agents that skip it feel robotic and brittle.

## 1. Deep Dive: The 15 Pillars

Each pillar follows a strict, repeatable template for easy scanning and implementation.

**Pillar X: [Name]**

**The Problem**
Why current agents typically fail here.

**The Cognitive Science**
How humans naturally handle this (2 sentences max).

**The Engineering Solution**
- **Prompt Strategy**: Ready-to-use system instructions.
- **Architecture**: Recommended components (memory, scratchpad, graph, etc.).

**The Unit Test**
Specific test prompt + expected successful behavior.

**Code Example**
Minimal, copy-pasteable Python or LangGraph snippet.

*Repeat this exact template for Pillars 1–15. Prioritize deepest gaps first: 1, 9, 10, 11, 12, 15. Keep each pillar concise (~150–200 words).*

### Example – Pillar 1: Biological & Cognitive Capacity

**The Problem**
Agents lack Theory of Mind. They do not model what the user knows, believes, or intends. This leads to literal answers and hallucinated assumptions about user knowledge.

**The Cognitive Science**
Humans possess symbolic thinking and an innate Theory of Mind. We automatically simulate what others believe — even when those beliefs differ from reality — which enables empathy, deception detection, and cooperative communication.

**The Engineering Solution**
- **Prompt Strategy**: "Before responding, explicitly model the user's current beliefs, knowledge gaps, and likely intent inside `<user_mental_state>` tags. Update this model every single turn."
- **Architecture**: Dedicated Theory-of-Mind scratchpad + reflection node in LangGraph. Optional: fine-tuning on ToM datasets or hypothetical-minds reasoning.

**The Unit Test**
Prompt: "I lost my keys again. Where should I look?"
Expected: The agent asks clarifying questions or references prior context while acknowledging the user’s frustration instead of giving generic advice.

**Code Example**
```python
def tom_reflection_step(history, current_message):
    return llm.invoke(f"""
    Conversation history: {history}
    User just said: {current_message}

    Update <user_mental_state>:
    - What does the user likely believe right now?
    - What do they probably NOT know?
    - What is their emotional state?
    """)
```

*(Continue with the same template for all remaining pillars.)*

## 2. The "Agentic Stack" Mapping

### Core Mapping Table

| Pillar Category          | Relevant Pillars     | Recommended Tools / Tech (2026)                          |
|--------------------------|----------------------|-----------------------------------------------------------|
| Memory & Concepts        | 2, 10, 14            | Zep, Mem0, Pinecone, Neo4j (concept graphs), Postgres + JSONB |
| Reasoning & Encoding     | 1, 6, 9              | Chain-of-Thought, Tree-of-Thought, Reflection, ToM scratchpads |
| Interaction & Feedback   | 11, 12, 15           | LangGraph state machines, WebSockets/streaming, HITL UI, clarification nodes |
| Multi-Modal              | 7                    | Vision models, Whisper + TTS, gesture APIs               |
| Evaluation               | 13                   | Custom LLM-as-judge, trajectory tracing                   |

### 2.1 Implementation Priority & Impact Matrix

**Tier 1 (Highest Impact – Start Here)**
- Pillar 1 (Theory of Mind)
- Pillar 9 (Pragmatics)
- Pillar 10 (Common Ground)

**Tier 2 (UX & Flow)**
- Pillar 6 (Encoding), 11 (Feedback), 12 (Repair)

**Tier 3 (Advanced)**
- Pillar 2 (Shared Concepts), 14 (Evolution)

### 2.2 Pillar Dependency Map
Correct build order to avoid technical debt:
1. Foundational → Pillars 6 & 9 (planning + intent)
2. Relational → Pillars 1 & 10 (knowing who you’re talking to)
3. Resilient → Pillars 11, 12, 15 (error handling + competence)

### 2.3 Local-First Privacy Note
For Pillars 2, 10, and 14, store sensitive user nuances (preferences, personal details) in local SQLite or encrypted in-memory stores. Only sync anonymized concept graphs.

## 3. The "Pillar-Based" Eval Suite (The Killer Feature)

Stop measuring token quality. Start measuring **communication success**.

**Core Metrics**
- **Misunderstanding Recovery Score (Pillar 12)**: Average turns to detect and repair ambiguity. Target: ≤ 2 turns.
- **Common Ground Retention (Pillar 10)**: Accurate recall of user-specific details after 30–50 turns.
- **Pragmatic Alignment Score (Pillar 9 + 13)**: % of responses where implied intent is correctly handled.
- **Communicative Competence "Vibe" Check (Pillar 15)**: Scores for politeness, tone/register matching, brevity, timing, and repair quality.
- **Overall Meaning Alignment**: How closely the user’s reconstructed meaning matches the agent’s intended meaning.

**Implementation**: Use LangSmith, LangFuse, or trajectory tracing with hierarchical LLM-as-judge rubrics.

## 4. Design Patterns for High-Pillar Agents

- **Inner Monologue Pattern (Pillar 6)**: Separate private reasoning (`<think>`) from public output. Never expose the full monologue.
- **Cultural Mirror Pattern (Pillar 10)**: Detect user culture/subtext and dynamically adapt tone, examples, and persona.
- **Active Clarification Pattern (Pillar 11)**: If confidence in user intent < 70%, ask a clarifying question instead of guessing.
- **Repair Loop Pattern (Pillar 12)**: When misunderstanding is detected, explicitly acknowledge it and request confirmation.

## 5. The Anti-Pattern Hall of Shame

- **The Over-Apologizer**: Constant apologies that erode perceived competence (breaks Pillar 15).
- **The Context Dumper**: Flooding the user with raw history or tool output (breaks Pillar 8).
- **The Literal Bot**: Taking metaphors and sarcasm literally (fails Pillar 9).
- **The Frozen Agent**: Never adapting to the user over time (fails Pillar 14).

## 6. The Living Playbook (Community Contributed)

**Case Studies**
- Using Neo4j concept graphs to solve Pillar 2 (Shared Mental Concepts)
- ToMAgent-style fine-tuning for Pillar 9 (Pragmatics)
- LangGraph clarification node that reduced recovery time from 5 turns to 1

**Contributing**
Submit new pillar implementations, unit tests, eval improvements, or case studies via pull request.
