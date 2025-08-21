# Building Effective Agents
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
	  ![[Pasted image 20250821025801.png]]
	- **Routing**: where the LLM classifies an input and directs it to a specialized followup task. This workflow allows for separation of concerns, and building more specialized prompts.
	  ![[Pasted image 20250821025717.png]]
	- **Parallelization**: LLMs can sometimes work simultaneously on a task and have their outputs aggregated programmatically. This is good for complex tasks or tasks require higher confidence result. 
		- Task Decomposition (for optimising speed of complex tasks)
		- Resampling (for higher confidence result)
	  ![[Pasted image 20250821025828.png]]
	- **Orchestrator - Workers**: An orchestrator LLM distributes tasks for worker LLMs to do, and then aggregates the result. This is useful for complex tasks that can be easily divided into subtasks.
	  ![[Pasted image 20250821025853.png]]
	- **Evaluator - Optimizer**: An evaluator LLM provides a solution and then receives feedback from an Optimizer LLM. Repeating this loop until meeting a certain criteria such as a maximum number of loops. This workflow is particularly effective when there is clear evaluation criteria, and when iterative refinement provides measurable value.
	  ![[Pasted image 20250821030056.png]]
- Agents
	- LLMs using tools based on environmental feedback in a loop.

[Introducing ambient agents](https://blog.langchain.com/introducing-ambient-agents/)

[How we built our multi-agent research system \\ Anthropic](https://www.anthropic.com/engineering/multi-agent-research-system)

[a-practical-guide-to-building-agents.pdf](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf)

[GitHub - humanlayer/12-factor-agents: What are the principles we can use to build LLM-powered software that is actually good enough to put in the hands of production customers?](https://github.com/humanlayer/12-factor-agents)

[Introducing the Model Context Protocol \\ Anthropic](https://www.anthropic.com/news/model-context-protocol)

Questions to answer:
1. Shall we build a agentic system or a workflow?
2. Shall we build a single agent system or a multi-agent system?