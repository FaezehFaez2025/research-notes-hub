# Automated Design of Agentic Systems

https://openreview.net/pdf?id=t9U3LW7JVX

## Summary

The paper introduces a new research direction called **Automated Design of Agentic Systems (ADAS)**, which aims to automatically design AI agents instead of relying on human-crafted workflows. The key idea is to represent an entire agent (including its prompts, reasoning steps, tool usage, and control flow) as executable code, and then use a powerful language model as a “meta-agent” to explore this space. Because code can express arbitrarily complex behaviors, this approach allows the system to search over a very large space of possible agent designs, going beyond prior work that only optimizes prompts. ([arXiv][1])

To realize this idea, the authors propose an algorithm called **Meta Agent Search**, where the meta-agent repeatedly generates new agent designs in code, evaluates them on tasks, and stores the best-performing ones in an archive that guides future improvements. Over time, this iterative “generate–evaluate–refine” process discovers increasingly effective and novel agent strategies. The main contribution is showing that such automatically discovered agents can outperform carefully hand-designed systems and even generalize well across different tasks and models, suggesting that AI systems can begin to design better AI systems on their own. ([OpenReview][2])

[1]: https://arxiv.org/abs/2408.08435 "Automated Design of Agentic Systems"
[2]: https://openreview.net/forum?id=t9U3LW7JVX "Automated Design of Agentic Systems"
