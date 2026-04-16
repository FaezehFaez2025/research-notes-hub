# MemSkill: Learning and Evolving Memory Skills for Self-Evolving Agents

- PDF: https://arxiv.org/pdf/2602.02474
- Code: https://github.com/ViktorAxelsen/MemSkill

## Summary

**Main Idea**

LLM agent memory systems typically rely on a small set of static, hand-designed operations (such as add, update, delete, and skip) for extracting, consolidating, and revising memory. These fixed procedures are inefficient on long histories and rigid under diverse interaction patterns.

**MemSkill** makes these memory operations **learnable and evolvable** by reframing them as an **evolving skill bank** — reusable skills that are continuously refined over training based on hard cases and failures. The result is a **self-evolving agent memory system** driven by interaction data.

![Figure 1: Prior turn-level handcrafted operations vs MemSkill span-level skill-conditioned generation](MemSkill1.png)

**Controller**

The controller is the only learnable component of the system. It selects a small set of relevant memory skills for the current context, implemented as a lightweight MLP trained with reinforcement learning. It operates in two steps:

1. Computes embeddings for the current state and for each skill separately using the same shared embedding model (i.e., Qwen3-Embedding-0.6B).
2. Scores each skill by comparing the state and skill embeddings, supporting a variable number of skills since the size of the skill bank may vary during training.

The controller is optimized using RL with rewards based on task-level performance such as answer correctness, encouraging skill selections that lead to better final outcomes.

**Executor**

The executor is responsible for executing the selected skill and generating outputs. It is implemented as a frozen large language model accessed via API, specifically LLaMA3.3-70B-Instruct during training and Qwen3-Next80B-A3B-Instruct for transfer experiments. Given the selected skill and the current context, the executor produces outputs such as reasoning steps, retrieved information, or final answers. The executor is not trained and serves as a fixed environment in the system.

**Designer**

The designer is responsible for generating and refining the set of available skills. It is implemented using an LLM-based prompting strategy that analyzes trajectories and failures to propose new or improved skills. The designer is fixed and not trained; it does not undergo optimization and instead relies on predefined prompting mechanisms to expand the skill set over time.

![Figure 2: MemSkill architecture overview (controller, executor, skill bank, designer, closed-loop optimization)](MemSkill2.png)

**Closed-Loop Optimization**

MemSkill operates in a closed-loop cycle where the controller selects skills, the executor applies them to generate outputs, and the system evaluates the results to compute rewards. These rewards are used to update the controller through reinforcement learning, while the designer can introduce new skills based on observed failures. This iterative loop enables continuous improvement in both skill selection and the overall system performance.
