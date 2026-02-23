## Creating and executing LLM plans
**Why Structure Matters**
- Plans need to be **parsed by downstream code**, not just read by humans
- Clear, unambiguous formatting enables reliable step-by-step execution
Plan format options (Best → Worst)
![[Pasted image 20260216121639.png]]
JSON Plan Pattern 
- System prompt: describe available tools + instruct "create a step-by-step plan in JSON format" 
- Output structure: a list of step objects, each containing: 
	- `description` — what the step does 
	- `tool` — which tool to call 
	- `arguments` — parameters to pass to that tool 
- Downstream code parses the JSON and executes each step sequentially
## Planning with code execution
**The Problem with Tool-Only Approaches**
- For data queries (e.g., spreadsheet Q&A), you end up creating **endless custom tools** (get_max, get_mean, filter_rows, get_unique, etc.)
- Every new edge-case query demands a new tool
- Result: **brittle, inefficient**, constant firefighting
**The Better Way: Let the LLM Write Code**![[Pasted image 20260216122230.png]]
- Instead of JSON plans executed step-by-step, prompt the LLM to **output executable code**
	- The code _is_ the plan, each line/block represents a plan step
	- Example: "What were the amounts of the last 5 transactions?"
		- LLM writes code that: load CSV → parse dates → sort by date → select last 5 → return price column. Done.
- LLMs already know how to use libraries like **pandas** (hundreds/thousands of built-in functions) from training data
- This gives the agent access to a massive function library without you hand-building each tool
**Performance Ranking** 
1. **Code** — best (richest expressiveness, leverages known libraries) 
2. **JSON** — good (parseable, unambiguous) 
3. **Plain text** — weakest
Caveats 
- **Sandbox execution** — running LLM-generated code ideally needs a safe environment 
- **Less developer control** — you don't know exactly what the LLM will do at runtime 
- **Not universal** — some applications still benefit from custom tool definitions 
- Planning outside of agentic coding still feels **cutting-edge and not fully mature**
## Multi-agentic workflows
**Why Multiple Agents?** 
- Same LLM, but decomposing work into **distinct roles** makes complex tasks easier to build and reason about 
- Analogies: 
	- One CPU, but we still use multiple processes/threads 
	- One task, but you'd hire a **team** with different roles, not one person
**Design Approach Ask:** 
- "What team of people would I hire to do this?" → each role becomes an agent
**Building Individual Agents**
- Each agent = an LLM prompted with a **specific role** + given **role-appropriate tools** 
	- Researcher → web search tools 
	- Graphic Designer → image generation, code execution for charts 
	- Writer → no extra tools needed (LLM text generation is sufficient)
**Two Communication Patterns**
1. Linear Pipeline (See linear communication pattern in [[#**The Two Most Common Patterns**|common communication patterns]])
	- Researcher → Graphic Designer → Writer → Final output ![[Pasted image 20260216122753.png]]
		- Simple, predictable 
		- Each agent's output feeds into the next
2. Manager/Coordinator Pattern (See Hierarchical (Single level) communication pattern in [[#**The Two Most Common Patterns**|common communication patterns]])
	- A **manager agent** (e.g., "Marketing Manager") is given the other agents as callable tools ![[Pasted image 20260216122813.png]]
	- Manager creates a plan, delegates tasks to agents, reviews final output 
	- Effectively 4 agents: 1 coordinator + 3 specialists 
	- Very similar to tool-use planning, but **tools are replaced with agents**
**Benefits**
- **Focus** — build/improve one agent at a time 
- **Parallelism** — team members can work on different agents simultaneously 
- **Reusability** — a well-built agent (e.g., graphic designer) can be reused across marketing brochures, social media, web pages, etc.
**Key Design Decision** 
- Choosing the [[#Communication patterns for multi-agent systems|communication pattern]] between agents is one of the most important architectural choices in multi-agent systems.
## Communication patterns for multi-agent systems
##### **The Two Most Common Patterns**
1. **Linear**![[Pasted image 20260216123321.png]]
	- `Researcher → Graphic Designer → Writer → Output`
		- Each agent passes output to the next in sequence
		- Simple, predictable, easy to implement
2. **Hierarchical (Single level)**![[Pasted image 20260216123653.png]]
	- Manager delegates tasks, receives reports back, coordinates flow
	- Implementation tip: have agents report back to the manager rather than passing directly to each other
##### **Advanced Patterns (Less common)**
1. **Deep Hierarchy**![[Pasted image 20260216123725.png]]
	- Some agents have their own sub-agents
	- Much more complex; used less often today
2. **All-to-All**![[Pasted image 20260216123736.png]]
	- Every agent can message any other agent at any time
	- Messages get added to the receiver's context; they respond when ready
	- Terminates when all agents declare "done" or a designated agent (e.g., writer) says output is ready
	- **Pros:** maximum flexibility and collaboration
	- **Cons:** hard to predict, chaotic, less controllable
	- Best for applications where you can **tolerate unpredictability** (e.g., creative tasks you can just re-run)
###### **Summary**
- Many multi-agent frameworks exist to make implementing these patterns easier. Worth exploring when building your own systems.![[Pasted image 20260216124021.png]]