Essentially **repos nested inside a repo**. Think of it like a dependency pinned to an exact commit SHA. When you clone a repo with submodules, you get the reference but have to explicitly init/update them to pull the actual code.
## Commands
##### `git submodule init`
- Registering a module in your local `.git/config`
	- Reads `.gitmodules`
	- Copies the submodule URL and path mappings into your local `.git/config`
	- Does **not** clone or checkout any code
##### `git submodule update`
Where the work happens
- Looks at the commit SHA which is pinned for each repo, and checks out that exact commit into the submodule's directory
- Usually in detached HEAD state.
	- **`--init`** — Runs `init` first if needed, so `git submodule update --init` is the common one-liner that does both steps.
	- **`--recursive`** — Handles submodules within submodules (nested). `git submodule update --init --recursive` is the "just make everything work" command you'll see in most READMEs.
	- **`--remote`** — Instead of checking out the commit the parent repo has pinned, it fetches and checks out the **latest commit on the tracked branch** (usually `main`). This is how you'd pull in upstream updates before committing a new pointer in the parent repo.
	- **`--merge`** or **`--rebase`** — Instead of detached HEAD, these integrate the updated submodule commit into your current local branch within that submodule via merge or rebase respectively. Useful if you're actively developing inside the submodule.
