### `git commit`
- `--amend`: replaces the most recent commit with a new one. 
	- Instead of creating a new commit, it combines your currently staged changes with the previous commit and lets you edit the commit message. 
- `--no-edit`: keeps the existing commit message without opening an editor. 
	- Most commonly paired with `--amend` — so `git commit --amend --no-edit` lets you add staged changes to the last commit while keeping its original message.
### `git checkout`
- `--ours <path>`: Only makes sense during a merge conflict. For the conflicted files in this path, keep _our_ version and throw away the incoming changes.
## `git merge`
- `ours`: Two ways to use, they do very different things:
	- `-s`: **strategy** — records the merge but keeps your tree **exactly** as-is. Nothing from the other branch is applied.
	- `-X`: **option** — auto-resolves **content conflicts** by picking your side. Does NOT handle modify/delete or rename conflicts.
### `git clean`
Remove files that are **untracked**.
### `git reset`
![[Pasted image 20260217113901.png]]
