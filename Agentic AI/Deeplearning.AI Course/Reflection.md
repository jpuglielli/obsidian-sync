## Reflection to improve outputs of a task
- Coming back from before, maybe Reasoning models are generally better for the reflection step?
- Reasoning models are good at finding bugs, so we can direct generate the first draft of code, then use a reasoning model to check for bugs![[Pasted image 20260213191716.png]]
- Getting external feedback (meaning new information outside of LLM, reflection becomes a **lot more powerful**![[Pasted image 20260213191828.png]]
## Why not just direct generation?
- n shot prompting is when you provide n examples of what you want the output to look like in the prompt
- Examples Andrew gives for tasks where reflection works better:![[Pasted image 20260213192456.png]]
- Tips for writing reflection prompts:
	1. Clearly indicate that you want it to review or iterate on the previous version of the output.
	2. Specify criteria to check, to guide the LLM to better reflect and critique the criteria you care most about
	3. Good way to learn how to write better prompts is to read prompts that others have written. Download open source software and go and find the prompts in a piece of software just to go read the prompts the author has written
- There are some tasks where reflection does not have much of benefit, and others where it offers a ton. 
## Chart Generation Workflow
![[Pasted image 20260213193351.png]]
## Evaluating the impact of reflection
- **Objective evals** -- Code-based evals are easier. Build a dataset of ground truth examples. Then run the workflow w/wo reflection and with different prompts and see how the performance changes![[Pasted image 20260213201700.png]]
- **Subjective Example**: Use LLM as a judge. Rubric based judging is better
	- **Bad example**: Can ask them to compare (e.g. which one is better, or give criteria), but there are issues with this. Answers often not that good. Position bias, will often choose the first input.
	- **Good example**: Give them a rubric. LLMs are not good at 1-5 rating, but good at binary ratings![[Pasted image 20260213202114.png]]
## Using external feedback
- Using external feedback is much more powerful than using the LLM as the only source of feedback![[Pasted image 20260213230124.png]]
- An example would be to execute the code the LLM generated, and feed the output/errors of that back to the LLM. Other examples:![[Pasted image 20260213230402.png]]
- 