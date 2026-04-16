# MemSkill: Learning and Evolving Memory Skills for Self-Evolving Agents

- PDF: https://arxiv.org/pdf/2602.02474
- Code: https://github.com/ViktorAxelsen/MemSkill

## Summary

### Main Idea

LLM agent memory systems typically rely on a small set of static, hand-designed operations (such as add, update, delete, and skip) for extracting, consolidating, and revising memory. These fixed procedures are inefficient on long histories and rigid under diverse interaction patterns.

**MemSkill** makes these memory operations **learnable and evolvable** by reframing them as an **evolving skill bank** — reusable skills that are continuously refined over training based on hard cases and failures. The result is a **self-evolving agent memory system** driven by interaction data.

![Figure 1: Prior turn-level handcrafted operations vs MemSkill span-level skill-conditioned generation](MemSkill1.png)

### Controller

The controller is the only learnable component of the system. It selects a small set of relevant memory skills for the current context, implemented as a lightweight MLP trained with reinforcement learning. It operates in two steps:

1. Computes embeddings for the current state and for each skill separately using the same shared embedding model (i.e., Qwen3-Embedding-0.6B).
2. Scores each skill by comparing the state and skill embeddings, supporting a variable number of skills since the size of the skill bank may vary during training.

The controller is optimized using RL with rewards based on task-level performance such as answer correctness, encouraging skill selections that lead to better final outcomes.

### Executor

The executor is responsible for executing the selected skill and generating outputs. It is implemented as a frozen large language model accessed via API, specifically LLaMA3.3-70B-Instruct during training and Qwen3-Next80B-A3B-Instruct for transfer experiments.

### Designer

Uses an LLM to refine existing skills and propose new ones based on failures.

### Closed-Loop Optimization

MemSkill alternates between (i) learning to select and apply skills to build memory banks and (ii) evolving the skill bank based on hard cases mined from recent training steps.

Each cycle begins with controller training on the current skill bank, during which the executor constructs memories and the system accumulates challenging cases.

The designer then updates the skill bank using representative hard cases.

The next cycle resumes controller training on the updated skill bank.

![Figure 2: MemSkill architecture overview (controller, executor, skill bank, designer, closed-loop optimization)](MemSkill2.png)

