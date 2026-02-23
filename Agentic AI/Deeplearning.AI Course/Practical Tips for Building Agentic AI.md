## Evaluations (Evals)
- **Iterate on both the system AND the evals** — both improve over time
- Eval Development Loop
	1. - Build prototype → run ~10-20 examples → manually inspect outputs
	2. Identify common failure modes (e.g., wrong dates, text too long)
	3. Build a small eval (10-20 examples) targeting that failure mode
	4. Use eval to track progress as you tweak prompts/algorithms
	5. Expand eval set over time as you discover gaps
- Two 'axes' of evaluation:![[Pasted image 20260215172331.png]]
- Tips
	- **Start small** — don't let "eval paralysis" slow you down; 10-20 examples is fine
	- **Blend human review + metric evals** — shift trust toward metrics as evals mature
	- **Fix evals when they disagree with your judgment** — if you know the system improved but the score didn't, your eval needs updating
	- **Benchmark against expert humans** — where the agent underperforms a human expert is where to focus next
## Error analysis and prioritizing next steps
- **Important skill!** And Important topic to get better at
- Agentic workflows have many components; **gut-feel prioritization** often leads to months of wasted effort
	- **Disciplined error analysis** is one of the biggest predictors of how fast a team improves their system
- **How to do error analysis**
	1. Set aside examples where output is fine, only analyze where the system underperformed
	2. Focus energy of components which are generating errors. If you have ideas on how they can be improved, worth spending more energy on those components rather than the ones you don't know how to improve
	3. Develop a habit of looking at traces. examining intermediate outputs after each step.
	4. **Compare to human expert**, at each step, ask "would a human have done significantly better given the same input?"
	5. **Don't blame downstream steps** for upstream failures (e.g., if search results were bad, don't blame the LLM for picking bad sources from bad options)
- Building error analysis with a spreadsheet
	- Follow the rules above![[Pasted image 20260215181808.png]]
## More error analysis examples
1. Invoice processing![[Pasted image 20260215182710.png]]![[Pasted image 20260216092504.png]]
2. Customer email response![[Pasted image 20260216092539.png]]![[Pasted image 20260216092605.png]]
- Error analysis tells you **which component** to focus on
- End-to-end evals alone aren't enough, add **component-level evals** for the specific step you're improving
	- Use info from the traces?
## Component level evaluations
- Why not just E2E evals?
	- Rerunning the **entire workflow** for every small change is expensive
	- Noise from other components' randomness can **mask small improvements** in the component you're tuning
- **Example: evaluating web search quality**
	- Create a list of **gold standard web resources** for a handful of queries
	- Run the web search component in isolation
	- Use standard information retrieval metrics (e.g., **F1 score**) to measure overlap between returned results and gold standard
- You can rapidly test by tuning hyperparameters
- Benefits of Component-Level Evals
	- **Clearer signal** — isolates whether the specific component is improving, free from noise of other steps
	- **Faster iteration** — no need to rerun the full pipeline for every tweak
	- **Team scalability** — individual teams can optimize their own component with a focused metric
- Always validate with an end-to-end eval** before calling it done, confirming that component improvements actually translate to better overall system performance
## How to address problems you identify
1. Improving non-LLM based components![[Pasted image 20260216093516.png]]
2. Improving LLM based components
	- Andrew tends not to fine-tune a model until I've really exhausted the other options, because fine-tuning tends to be quite complex. 
		- But for applications where after trying everything else and are still at 90% - 95% performance and he needs to eke out those last few percentage points of improvement, then sometimes fine-tuning is a great technique
		- Only on mature projects because of how costly it is
	- The more you try out new models, the better your intuition for which models are better at which tasks
	![[Pasted image 20260216095436.png]]
- Developing intuition for model intelligence
	- Andrew spends a lot of time reading other people's prompts online, or open source packages
	![[Pasted image 20260216095856.png]]
## Latency & cost optimization
- Optimize output quality first, then latency and cost later
- Optimizing latenct
	- Start by benchmarking the time it takes for the workflow to execute
		- Research agent example:![[Pasted image 20260216100309.png]]
- Optimizing cost
	- Costing workflow example![[Pasted image 20260216100439.png]]
- TLDR by benchmarking cost and/or latency, we can see the components which might be worth focusing on to improve
## Development process summary
Two Core Activities 
1. **Building** — writing code, improving the system 
2. **Analyzing** — error analysis, reading traces, building evals, deciding *where* to build next 
> Analysis often doesn't feel like progress, but it's equally important as building.![[Pasted image 20260216100832.png]]
- You **bounce back and forth** between stages — it's not a linear process.
Common Mistake
- Less experienced teams spend **too much time building, not enough analyzing** 
- Analysis is what tells you where building effort will actually pay off
On Tooling 
- Many observability/monitoring tools exist (traces, costs, runtime logging) and can help 
- But most agentic workflows are **custom enough** that you'll end up building **custom evals** tailored to your specific failure modes 
- Use both: off-the-shelf tools + custom evals

		