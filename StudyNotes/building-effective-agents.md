# Building Effective Agent Systems

[Building Effective AI Agents \\ Anthropic](https://www.anthropic.com/engineering/building-effective-agents)
- Workflow vs Agent
	- Workflow: systems where where LLMs and tools are orchestrated to follow a pre-defined path
	- Agent: where LLMs dynamically creates their own processes and tool usage, maintaining control over how they solve tasks.
- When to use a Workflow/Agent
	- Workflow: when the steps of accomplish a task is clear. Workflows offer better predictability and consistency for well-defined tasks.
	- Agent: when the steps need to be dynamically decided by the agent. Agents are better option for tasks that require flexibility and model-driven decision-making at scale.
- Building Block
	- Augmented LLM: an LLM augmented with access to external tools/knowledge base/memory
- Workflows
	- **Prompt Chaining**: where the task solving process is divided into a sequence steps. The output put of the previous steps will be the input of the following steps. This workflow is ideal for situations where the task can be easily and cleanly decomposed into fixed subtasks.
	  
	  ![](../Images/chain.png)
	- **Routing**: where the LLM classifies an input and directs it to a specialized followup task. This workflow allows for separation of concerns, and building more specialized prompts.
	  ![](../Images/routing.png)
	- **Parallelization**: LLMs can sometimes work simultaneously on a task and have their outputs aggregated programmatically. This is good for complex tasks or tasks require higher confidence result. 
		- Task Decomposition (for optimising speed of complex tasks)
		- Resampling (for higher confidence result)
	  ![](../Images/parallelization.png)
	- **Orchestrator - Workers**: An orchestrator LLM distributes tasks for worker LLMs to do, and then aggregates the result. This is useful for complex tasks that can be easily divided into subtasks.
	  ![](../Images/orche-work.png)
	- **Evaluator - Optimizer**: An evaluator LLM provides a solution and then receives feedback from an Optimizer LLM. Repeating this loop until meeting a certain criteria such as a maximum number of loops. This workflow is particularly effective when there is clear evaluation criteria, and when iterative refinement provides measurable value.
	  ![](../Images/gen-eval.png)
- Agents
	- LLMs using tools based on environmental feedback in a loop.
- Core Principles
	- Maintain **simplicity** in your agent's design.
	- Prioritize **transparency** by explicitly showing the agent’s planning steps.
	- Carefully craft your agent-computer interface (ACI) through thorough tool **documentation and testing**.


[How we built our multi-agent research system \\ Anthropic](https://www.anthropic.com/engineering/multi-agent-research-system)
- Benefit of multi-agent system
	- Works better for open-ended tasks like research, where no pre-defined pipeline is provided. 
	- Maintain multiple threads of investigation independently.
	- Easily scale performance through subagents collaborating and working in parallel.
- Why multi-agent systems work?
	- More tokens spent (main reason)
	- number of tool calls
	- model choice
- Downsides of multi-agent systems
	- Burning tokens fast (costly)
	- Not suitable for tasks that require all the subagents to share the same context or involve heavy dependency between subagents (e.g. coding). It's suitable for tasks that require a lot of parallelised work. 
- Architecture Overview
	- Orchestrator - Worker
	- A lead agent coordinates the process and delegates tasks to subagents that work in parallel.
	- ![](../Images/anthropic-research-agent.png)
- Prompting Strategy
- Evaluation
- Engineering Challenges
- Other Findings


[Introducing ambient agents](https://blog.langchain.com/introducing-ambient-agents/)



[a-practical-guide-to-building-agents.pdf](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf)

[GitHub - humanlayer/12-factor-agents: What are the principles we can use to build LLM-powered software that is actually good enough to put in the hands of production customers?](https://github.com/humanlayer/12-factor-agents)

[Introducing the Model Context Protocol \\ Anthropic](https://www.anthropic.com/news/model-context-protocol)
[Introduction - Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro)

[Extended Thinking \ Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking#interleaved-thinking)

[anthropic-cookbook/patterns/agents/prompts at main · anthropics/anthropic-cookbook · GitHub](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents/prompts)
[Rainbow Deploys with Kubernetes \| Brandon Dimcheff](https://brandon.dimcheff.com/2018/02/rainbow-deploys-with-kubernetes/)

Questions to answer:
1. Shall we build a agentic system or a workflow?
2. Shall we build a single agent system or a multi-agent system?
3. How to build the tool system/agent-tool interface?