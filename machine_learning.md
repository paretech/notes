# Resource Recommendations

- [Machine Learning CS229 Andrew Ng (Coursera TBR)](https://www.youtube.com/watch?v=jGwO_UgTS7I)
- https://www.andrewng.org/courses/
- [Designing Machine Learning Systems: An Iterative Process for Production-Ready Applications 1st Edition
by Chip Huyen (Author)](https://www.amazon.com/Designing-Machine-Learning-Systems-Production-Ready/dp/1098107969)
- [Machine Learning Design Patterns: Solutions to Common Challenges in Data Preparation, Model Building, and MLOps 1st Edition
by Valliappa Lakshmanan (Author), Sara Robinson (Author), Michael Munn (Author)](https://www.amazon.com/_/dp/1098115783)
- [Deep Learning (The MIT Press Essential Knowledge series) Illustrated Edition
by John D. Kelleher (Author)](https://www.amazon.com/dp/0262537559)
- [Tensorflow Playground](https://playground.tensorflow.org/)
- https://developers.google.com/machine-learning/intro-to-ml
- https://mathacademy.com/
- https://course.fast.ai/
- https://www.statlearning.com/
- Papers
  - [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
  - [Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/abs/1301.3781)
- Podcasts
  - https://www.youtube.com/@ToolUseAI
    - [Use AI To Build Your Own Tools (ft Manuel Odendahl) - Ep 40](https://www.youtube.com/watch?v=dVJ59dDHoVk)
- primitive recursive functions vs general recursive functions
- sycafancy
- Prompt Steering
  - If there is a better way, tell me by providing alternatives and explaining why.
  - Correct me when I'm wrong. Don't appologize and explain how I'm wrong.
  - Provide a migration plan from <a> to <b> by providing a step-by-step approach to allow incremental migration.

Write a "production-ready" docstring for <target> using the Google style guide. The docstring includes:

1.  **Summary line**: A concise one-line description of what the function does
2.  **Extended description**: More detailed explanation of the function's behavior and logic
3.  **Args section**: Detailed description of each parameter with types and purposes
4.  **Returns section**: Description of the return value structure and contents
5.  **Raises section**: Documentation of the exception that can be raised

## ML/AI Applications

- Dictation
- MacWhisper

## LLM Multi-Agent Workflows and Techniques

- https://blog.sshh.io/p/how-i-use-every-claude-code-feature/comments
  - This was the post that started it all for me. Be sure to checkout the comments, author addresses several questions I had from reading post.
- https://blog.sshh.io/p/building-multi-agent-systems
- https://blog.sshh.io/p/building-multi-agent-systems-part
- https://www.anthropic.com/engineering/multi-agent-research-system
- https://arxiv.org/abs/2308.00352
- https://code.claude.com/docs/en/sub-agents
- https://github.com/humanlayer/humanlayer/blob/main/.claude/commands/create_plan.md
- https://github.com/li0nel/claude-loop
- https://www.anthropic.com/engineering/building-effective-agents
  - "When building applications with LLMs, we recommend finding the simplest solution possible, and only increasing complexity when needed. "
  - "Agentic systems often trade latency and cost for better task performance, and you should consider when this tradeoff makes sense."
  - "Prompt chaining decomposes a task into a sequence of steps, where each LLM call processes the output of the previous one."
  - "[Prompt chaining is] ideal for situations where the task can be easily and cleanly decomposed into fixed subtasks. The main goal is to trade off latency for higher accuracy, by making each LLM call an easier task."
- https://github.com/anthropics/claude-cookbooks/tree/main/patterns/agents
