LangChain is an open-source framework for LLM applications. 
- Python and TS packages. 
	- Focuses on composition and modularity
- Key value adds:
		- Modular components
		- Use cases: common ways to combine components
- It provides a structured way to connect LLMs to external data sources, tools, and APIs. 
- Essentially giving you composable building blocks for creating complex AI-powered workflows.
## Core Ideas

At its heart, LangChain addresses a fundamental challenge: LLMs on their own are stateless text-in/text-out systems. 
- To build useful applications, you typically need to orchestrate multiple steps  
	- retrieving context
	- calling APIs
	- maintaining conversation history
	- making decisions about what to do next, etc. 
LangChain gives you abstractions to wire all of that together.

## Key Components

**Models** — Unified interfaces for interacting with various LLM providers (OpenAI, Anthropic, local models, etc.), so you can swap providers without rewriting your application logic.

**Prompts** — Templating and management for constructing prompts dynamically, including few-shot examples and output parsing.

**Chains** — The original core abstraction — sequential pipelines where the output of one step feeds into the next. Think of it like Unix pipes but for LLM operations.

**Agents** — More autonomous systems where the LLM decides _which_ tools to call and in what order, rather than following a fixed sequence. The model acts as a reasoning engine that plans and executes actions.

**Retrieval (RAG)** — Built-in support for Retrieval-Augmented Generation, connecting LLMs to vector stores, document loaders, and text splitters so models can answer questions grounded in your own data.

**Memory** — Mechanisms for persisting state across interactions, enabling multi-turn conversations and long-running workflows.

## The Ecosystem

LangChain has evolved into a broader ecosystem:

- **LangChain Core** — the base abstractions and LCEL (LangChain Expression Language), a declarative way to compose chains
- **LangGraph** — a newer library for building stateful, multi-actor agent workflows as graphs (nodes and edges), giving you more control than traditional agents
- **LangSmith** — an observability/debugging platform for tracing, evaluating, and monitoring LLM application runs
- **LangServe** — for deploying chains as REST APIs

## Where It Fits

LangChain sits in the "orchestration layer" between your application code and the LLM providers. 
- It's not a model itself — it's the glue and plumbing. 
- It's particularly useful when you're building RAG systems, tool-using agents, multi-step reasoning pipelines, or chatbots that need to interact with external systems.

## Worth Knowing

The framework has been both widely adopted and widely debated. Critics point out that its abstractions can add unnecessary complexity for simple use cases, and the API surface has changed rapidly. The team has responded by simplifying the core library and investing heavily in LangGraph for the agentic use cases that are increasingly dominant. For straightforward LLM calls, you often don't need LangChain at all — but for complex orchestration, it provides a lot of useful scaffolding out of the box.