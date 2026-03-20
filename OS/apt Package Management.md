`apt` is to Ubuntu what `pip` is to Python — it downloads, installs, and manages software packages for the OS.
## Core Mental Model
Three layers:
1. **Sources** (where to look) → `/etc/apt/sources.list` and `/etc/apt/sources.list.d/`
2. **Index** (what's available) → refreshed by `apt update`
3. **Installed** (what's on disk) → changed by `apt install/remove/upgrade`
> Most problems come from layer 1 (wrong/missing repo) or not pinning at layer 3.
## How Repos Work
- Repos are remote servers hosting `.deb` files + an index (catalog of available packages and versions)
- `apt update` refreshes your **local copy of the index** — it doesn't update any software
- Ubuntu ships default repos, but you add third-party ones (Docker, etc.) when the default version is outdated
- Adding a repo = dropping a `.list` file in `/etc/apt/sources.list.d/`
## Version Pinning
Unpinned installs in Dockerfiles are non-reproducible:

```dockerfile
# BAD — gets whatever version exists today
RUN apt-get update && apt-get install -y nginx

# GOOD — deterministic builds
RUN apt-get update && apt-get install -y nginx=1.24.0-1ubuntu1
```
Same principle as `package-lock.json` or `poetry.lock`.

**Tradeoff:** repos rotate out old versions, so pinned versions can break over time. For critical infra, use snapshot repos or pin to major versions.
![[Pasted image 20260318202051.png]]