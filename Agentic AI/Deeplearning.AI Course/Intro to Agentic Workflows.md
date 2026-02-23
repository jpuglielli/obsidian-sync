https://github.com/https-deeplearning-ai/agentic-ai-public
## What is Agentic AI?
- ...
## Degrees of Autonomy
- On a spectrum based on the number of decisions the LLM makes
- More autonomy -> more unpredictable
## Benefits of Agentic AI
- Much better performance for LLMs
- Can parallelize work
- Modular: Can add or swap out tools, LLM
## Agentic AI Applications
- More suited to tasks dealing with pure text, well defined set of steps as well as standard procedure to follow
## Task Decomposition
- Take the task, and decompose it into steps. If the output is unsatisfactory, take the existing steps and subdivide as needed
- Workflows are made of building blocks:

| Models | LLMS           | Text generation, tool use, info extraction |
| ------ | -------------- | ------------------------------------------ |
|        | Other models   | PDF -> Text, Image analysis                |
| Tools  | API            | Web search, check email...                 |
|        | Info Retrieval | DBs, RAG                                   |
|        | Code Execution | Calc, data analysis...                     |
- We can compose these building blocks into steps
- **Think**, can each step be implemented with either an LLM or a [[Tool Use|tool]] call? If no:
	- Think how would I, as a human, do this step. Can I decompose steps further in such a way that the sub-steps can be implemented via LLM or tool
## Evaluating Agentic AI (Evals)
- Very difficult in advance to know where things will go wrong, so we build first then figure out where it is not satisfactory, and come up with ways to evaluate the output
- **Objective** evals (Code as judge) or **Subjective** evals (LLM as judge)
- E2E and component evals
- Good to examine the intermediate outputs (traces) of the LLM to understand where it is falling short of expectations
- Specific evals:
	- [[Reflection#Evaluating the impact of reflection]]
## Agentic Design Patterns
4 major design patterns, more in depth later in course
1. [[Reflection]]
	1. Feed agents outputs or results from other systems back in as feedback, allowing to iterate on this feedback
2. [[Tool Use]]
	1. The agent calls external tools (search, code execution, APIs, etc.) to gather information or take actions it can't do with text generation alone.
3. Planning
	1. The agent decomposes a complex goal into a sequence of smaller steps before executing them.
	2. Harder to control??
4. Multi-agent collaboration
	1. Multiple specialized agents work together, each handling a different role or subtask, to solve problems that are too complex for a single agent.
	2. Harder to control since we don't know what the agents will do