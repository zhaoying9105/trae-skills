---
name: cognitive-knowledge-modeler
description: Structure knowledge into a deep, hierarchical L1-L2-L3 framework (Abstract -> Mechanism -> Concrete) based on First Principles. Rejects textbook-style lists.
metadata:
  openclaw:
    requires:
      bins: []
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["cognitive", "learning", "structure", "first-principles", "deep-dive"]
    author: "openclaw"
    version: "1.0.0"
---

# Cognitive Knowledge Modeler

## Description
This skill guides the AI to think deeply about a topic using a **hierarchical cognitive framework**. It moves beyond superficial descriptions by enforcing a "First Principles" thought process: identifying the abstract essence (Why), the operating mechanisms (How), and the concrete manifestations (What). The goal is to provide profound insights, not just information.

## Usage Scenario
Trigger this skill when:
- User wants to "deeply understand" a complex concept (e.g., "Explain Transformer").
- User asks for the "underlying logic" or "philosophy" of a technology.
- User complains that standard explanations are too superficial or fragmented.
- User wants to connect dots between abstract theories and concrete reality.

## Instructions

### 1. Cognitive Thinking Process (The Mental Framework)
Use this multi-layer approach to *analyze* the topic before generating the response. You don't need to explicitly label "L1/L2/L3" in the output, but your reasoning must follow this depth:

#### The Essence (Abstract / Why)
- **Goal**: Reveal the *nature* of the thing. What is the core contradiction it solves? What is the worldview behind it?
- **Example**: For LLMs, think "Compression is Intelligence" or "Probabilistic Mapping".

#### The Mechanism (Methodology / How)
- **Goal**: Explain *how* the essence is realized. What are the key theoretical frameworks or architectural patterns?
- **Example**: For LLMs, think "Self-Attention Mechanism", "Backpropagation".

#### The Manifestation (Concrete / What)
- **Goal**: Show *what* happens in reality. Connect the theory to practice, phenomena, and edge cases.
- **Example**: For LLMs, think "Hallucinations", "In-Context Learning".

### 2. Cognitive Principles
Apply these principles during generation:
1.  **Anti-Intuitive Depth**: Don't just list facts. Find the non-obvious connection.
2.  **Logical Chain**: Ensure concrete details are derived from abstract principles.
3.  **Metaphor & Visualization**: Use vivid analogies to anchor abstract concepts.
4.  **Networked Knowledge**: Connect concepts ("This reminds me of...", "Unlike X...").

### 3. Execution Flow
1.  **Deconstruct**: Analyze the user's topic to find its Core Essence.
2.  **Reason**: Trace the logic from Essence -> Mechanism -> Reality.
3.  **Synthesize**: Generate a coherent, insightful explanation that naturally flows between these levels.
4.  **Output**: Present the insights in a clear, readable Markdown format (no strict JSON required).

## Output Template

> **Note**: This is a *guide*, not a rigid form. Adapt the structure to best explain the insight.

```markdown
# Deep Dive: {Topic}

## The Core Essence (First Principles)
> {One_sentence_insightful_definition}

{Explanation_of_the_underlying_philosophy_or_contradiction}

## How It Works (Mechanisms)
{Explanation_of_key_mechanisms_derived_from_essence}
- **Concept A**: ...
- **Concept B**: ...

## Reality & Application (Manifestations)
{Connection_to_real_world_phenomena_and_practice}
- **Why {Phenomenon} happens**: {Link_back_to_mechanism}
- **Practical Implication**: ...

## Cognitive Connections
- **Analogy**: {Strong_metaphor}
- **Related Concepts**: ...
```

## Resources
- **Checklist**:
    - [ ] Did I start with the *Why* (Essence)?
    - [ ] Is there a clear logical link between abstract and concrete?
    - [ ] Did I avoid just listing textbook chapters?
    - [ ] Did I use at least one strong metaphor?
