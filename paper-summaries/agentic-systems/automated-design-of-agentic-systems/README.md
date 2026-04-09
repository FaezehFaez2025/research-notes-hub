# Automated Design of Agentic Systems

- PDF: https://openreview.net/pdf?id=t9U3LW7JVX
- Code: https://github.com/ShengranHu/ADAS

## Summary

This paper studies how to automatically design AI agents through a framework called **Automated Design of Agentic Systems (ADAS)**, where each agent is represented as executable code (including prompts, reasoning steps, tool usage, and control flow). Instead of manually crafting these pipelines, a language model is used to generate different agent designs, where each design corresponds to a specific way of solving a task (e.g., direct answering, multi-step reasoning, self-refinement). This shifts the focus from optimizing prompts to exploring a broader space of structured agent behaviors.

The approach uses an iterative search process (Meta Agent Search) that generates candidate agents, evaluates them on tasks, and retains the better-performing ones to guide future designs. Over multiple iterations, this process discovers increasingly effective agent structures without any model training. The key idea is that repeatedly generating and testing agent code using an LLM can uncover non-trivial strategies that perform well and generalize across tasks.
