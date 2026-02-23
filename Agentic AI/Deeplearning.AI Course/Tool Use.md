## What are tools
- Simple tool execution (dotted line means that it is up to the LLM to decide whether to use 0-n of the available tools):![[Pasted image 20260214110122.png]]
- Examples ![[Pasted image 20260214110324.png]]
- How LLMs choose between tools? TBD Multiple tools example: ![[Pasted image 20260214110509.png]]
## Creating a tool
- How LLMs trigger tool calls, before they were explicitly trained to call tools? Note that the developer has to write code to take the output of `get_current_time` and feed it back into the conversation history:![[Pasted image 20260214110902.png]]
- Example with arguments:![[Pasted image 20260214111013.png]]
- **There is a modern syntax for LLMs trained to use tools, so we do not need to do the above anymore**, see [[#Tool Syntax]]
## Tool Syntax
- Modern LLMs are pre-trained for tool-use capabilities; however, you must **register** each tool using a specific **schema** or syntax for the model to recognize and invoke it correctly.
## Code Execution
- Instead of building discrete tools for every possible function (like square roots or interest calculations), you can empower an LLM to **write and execute its own code** in a sandbox.
	- Self correct by returning the output/error codes via [[Reflection]]
- To avoid bad code execution, use sandbox environments ([[E2B]], [[Docker]])
## MCP
- See: [[Model Context Protocol]]
