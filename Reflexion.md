---
tags:
  - gpt
  - mas
  - AI
  - llm
Authors: Shinn et al.
Year: 2023
Publish: NeurIPS
---
Links: [paper](https://openreview.net/forum?id=vAElhFcKW6), [code, demo](https://github.com/noahshinn/reflexion)
annotation-target:: [[Reflexion]]

## Summary
Reflexion is a MAS framework that involves 3 distinct modules, which are Actor, Evaluator, and Self-reflector to improve existing models. Each of the modules could use a different model. 

## Main Ideas
### Modules
- Actor - specifically prompted to generate the necessary text and actions conditioned on the state observations.
	- LLM
- Evaluator - assessing the quality of the generated outputs produced by the Actor. It takes as input a generated trajectory and computes a reward score (a scalar value) that reflects its performance within the given task context.
	- Rule-based (reasoning task)
	- pre-defined heuristic (decision-making task)
	- LLM (decision-making and programming tasks)
- Self-reflector - Generating verbal self-reflections to provide valuable feedback for future trials, given the output of Actor and Evaluator.
	- LLM
- Memory - Contains the output of self-reflector.

### Results
- Decision making
	- Reflexion framework can easily identify an early mistake. The agent can even suggest a long-term plan.
	- Memory unit helps provide useful experience in searching for a room
	- Hand-written heuristic works better than GPT as an Evaluator.
	- Reflexion effectively reduces hallucinations and inefficient planning throught trials.
- Reasoning
	- The baseline models fail to improve accuracy through trials.
- Programming
	- Use LLM as Evaluator to generate unit test suite
	- Some experiments have a lot of FN and FP due to bad unit test generation
	- Evaluated with pass@1

## Experiment Setup
- Tasks & Datasets    
    - Multi-step decision making: ALFWorld
    - Reasoning: HotpotQA
    - Coding: MBPP, HumanEval, LeetcodeHardGym, their own dataset
- Models
    - Actor
        - ReAct
        - CoT
- Experiment
    - _I'm actually not sure if they are using GPT3 or 4 because it's unclear._
    - Decision making
        - Actor - GPT3 + ReAct
        - Evaluator - GPT3 + Hand written heuristic
        - Self-reflector - GPT3
        - Memory - 3 self-reflection
    - Reasoning
        - Actor
            - GPT3 + ReAct
            - GPT3 + CoT
            - GPT3 + CoT + Context
            - GPT3 + CoT + 1 memory
        - Evaluator - exact match of the answer, binary signal
        - Self-reflector - GPT3
        - Memory - 3 self-reflection
    - Programming
        - Actor
            - GPT4
        - Evaluator
            - GPT4 + CoT
        - Self-reflector
            - GPT4
        - Memory - 1 self-reflection
## Annotations
