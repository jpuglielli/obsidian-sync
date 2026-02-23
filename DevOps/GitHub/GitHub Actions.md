### What is GitHub actions?
- Platform to automate developer workflows
- What are those workflows?
## Github action jobs
- A **job** is a unit of work in a workflow. Each job runs on its own runner (machine) and contains a sequence of **steps**.
- **Jobs run in parallel by default.** If you define `build` and `deploy` as separate jobs, they start at the same time on separate runners unless you add `needs:`
	- **`needs:` creates ordering.** 
- **Each job gets a fresh environment.** 
	- **Each gets a fresh environment** — jobs don't share filesystems; use `outputs` or `artifacts` to pass data between them
- **Each runs on a runner**: you specify which machine via `runs-on:`
- **Contain sequential steps**: steps within a job share a filesystem and run one after another on the same runner
- **Can have their own settings**: each job can specify its own runner, container, environment variables, permissions, and concurrency
**When to use one job vs multiple:**
- Same job: steps that depend on shared files or need to run sequentially on the same machine
- Separate jobs: work that can run in parallel, needs a different runner/container, or represents a logical stage in a pipeline (e.g. build → test → deploy)
## Github actions runners
The machines that execute jobs
- **GitHub-hosted** — managed by GitHub, clean VM each time (e.g. `runs-on: ubuntu-latest`)
- **Self-hosted** — managed by your company, needed for internal network access, security, or custom hardware
- **Containers** — optionally run inside a Docker container on the runner for a consistent, reproducible environment