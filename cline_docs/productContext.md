# productContext.md

## What Is This Project?

`aligna` is a meta-framework for AI-native software collaboration.

It aims to reduce the need for repetitive prompting and manual instruction by embedding contextual knowledge directly within the project structure.

At the same time, it introduces the concept of **external reviewers**—humans or AI agents that provide oversight, validation, and feedback to ensure quality, guide decision-making, and constrain ungrounded behaviors.

## Why Does This Matter?

As AI systems become more capable, we face a fundamental challenge:
> How can we **control the quality** of autonomous AI activity within complex systems?

`aligna` addresses this by:
- Providing assistants with structured memory and navigation
- Delegating dynamic supervision to **reviewer agents** who can evaluate decisions, correct drift, and ensure adherence to purpose

## Who Is This For?

- Developers integrating AI into real software ecosystems
- Prompt engineers designing multi-agent workflows
- Reviewers (human or AI) assessing assistant outputs for accuracy, safety, or coherence
- Researchers exploring AI alignment through system-level design

## Design Principles

- **Static context guides active agents.**
- **Reviewers guide the agents.**
- **Bridge adapters connect agents to context.**
- **All logic is declarative and observable.**

## Key File Anchors

- `README.md`: Universal entrypoint
- `cline_docs/activeContext.md`: Current task state (bridge for Cline)
- `cline_docs/productContext.md`: Project identity and review philosophy

## Long-Term Goal

To enable reliable, explainable, tool-agnostic AI workflows grounded in:
- Declarative architecture
- Embedded context
- Independent review
- Promptless cooperation

## Reviewer Layer (Emerging)

This project invites the integration of **reviewer agents**—entities that:
- Evaluate outputs
- Guide assistants back toward goals
- Record decisions and interventions for traceability

This layer will be prototyped via:
- Explicit reviewer prompts
- Human-in-the-loop validation
- Feedback-driven context updates
