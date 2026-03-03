https://mcusercontent.com/fb53aa76bbc82533008812f2f/files/f7f6bd1b-712e-4674-0c63-b334f3cdd346/DevOps_Job_Requirements_Your_Path_to_a_High_Paying_Global_Career_v2.pdf

https://roadmap.sh/devops
https://confluence.teslamotors.com/pages/viewpage.action?spaceKey=POPS&title=Roadmap+of+Objectives
https://confluence.teslamotors.com/display/POPS/WIP%3A+Continuous+Integration%2C+Continuous+Delivery%2C+and+Site+Reliability+Engineering+Production+Readiness+Review
**Questions:**
- What to focus on, what can be skipped or skimmed
- What does he use the most in day-to-day work, should be learned deeply
- AI Usage. How much should I use?
	- I've gotten the advice below
- How to get fundamentals down? The plan I have in place really starts with docker. I know enough about Docker so that phase 2 should be easy.
- I have a hard time motivating when there is not a clear goal in mind. Another consideration is if there are any opportunities within Tesla to do this kind of work (slowly, but for free)
**Tentative Idea**
- Use AI for imlpementation
- Figure out high level architecture, and give very specific instructions. Leave opportunities to go in and code by hand later?
- If I see something that I do not understand, research it.
	- Active recall & Feynman technique, develop intuition

build locally → automate the build → deploy to cloud → codify the infrastructure → orchestrate → observe
## Phase 1: Foundation

**Goal:** Build the base layer everything else sits on.

### Linux Fundamentals

- Shell proficiency: pipes, redirects, subshells, exit codes, signal handling
- Filesystem & permissions: FHS, `/etc`, `/var`, `/proc`, `/sys`, `chmod`, `chown`, `umask`
- Process management: fork/exec, process states, signals, `systemd`, `journalctl`, cgroups, namespaces
- Text processing: `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `jq`
- Package management: `apt`/`yum`, users/groups, SSH config
- Disk management: `lsblk`, `df`, `mount`, LVM basics

### Bash Scripting

- Variables, conditionals, loops, functions
- Exit codes and error handling (`set -euo pipefail`)
- Input parsing, argument handling
- Cron jobs and automation
- Write real scripts — don't just do exercises

### Networking Fundamentals

- TCP/IP, DNS, HTTP/HTTPS, ports
- Tools: `curl`, `dig`, `ss`, `tcpdump`, `traceroute`
- Firewalls: `iptables`/`nftables`
- Subnetting, CIDR notation
- `/etc/hosts`, `/etc/resolv.conf`
- Understanding how a request flows from browser to server

### Git (Quick Review)

- Already used daily — fill in gaps only
- Focus areas: rebasing, cherry-pick, reflog, merge conflict resolution
- Branching strategies (trunk-based, GitFlow)
- Time box: 1-2 days max

**Suggested resources:**

- "How Linux Works" — Brian Ward (mental models)
- "The Linux Command Line" — William Shotts (free online)
- Julia Evans' zines (networking, Linux internals)

**Time box:** 2-3 weeks total for Phase 1.

---

## Phase 2: Containerization

**Goal:** Understand containers from the ground up, not just `docker run`.
### Docker

- What containers actually are (cgroups + namespaces + union filesystem)
- Dockerfile best practices: multi-stage builds, layer caching, minimal images
- Volumes, networking modes (bridge, host, none)
- Docker Compose for multi-service setups
- Image security scanning basics
- Build and containerize a real app (use your Django stack)

### Key Concepts
- Containers vs VMs — understand the actual difference at the kernel level
- OCI image spec basics
- Container registries (Docker Hub, ECR, GCR)

**Why this comes before cloud:** Docker runs locally, builds directly on Linux fundamentals (cgroups, namespaces), and everything after assumes you know it.

**Hold off on Kubernetes** — it comes in Phase 6.

---

## Phase 3: CI/CD

**Goal:** Automate the build-test-deploy cycle. This is the core of DevOps.

### Concepts

- What CI/CD actually solves (fast feedback, repeatable deployments)
- Build → Test → Package → Deploy pipeline stages
- Artifact management
- Environment promotion strategies (dev → staging → prod)

### Tools (pick one to start, understand the concepts to transfer)

- **GitHub Actions** — lowest friction, start here
- Jenkins — widely used in enterprise, good to understand
- GitLab CI — strong if your org uses GitLab

### Practice Project

- Wire up a real pipeline for a Django app:
    - Lint (ruff/flake8)
    - Run tests
    - Build Docker image
    - Push to container registry
    - Deploy to a target environment
- Add branch protection rules, required checks

**The only way to learn CI/CD is to build a pipeline.** Reading docs without doing it is useless.

---

## Phase 4: Cloud Basics

**Goal:** Understand cloud infrastructure by deploying real workloads.

### Pick One Provider

- **AWS** — bigger job market, more resources, recommended
- Azure — strong if your org uses Microsoft

### Core Services to Learn Deeply (AWS)

- **Compute:** EC2 (instances, AMIs, security groups, key pairs)
- **Storage:** S3 (buckets, policies, lifecycle rules), EBS
- **Networking:** VPC, subnets, route tables, internet gateways, NAT, security groups vs NACLs
- **Identity:** IAM (users, roles, policies — this is critical)
- **Database:** RDS (managed PostgreSQL)
- **Containers:** ECR (image registry), ECS or EKS

### Approach

- Do NOT chase certifications first. Build projects.
- Deploy your containerized Django app to the cloud manually
- Understand the networking: why can't my app reach the database? Why can't I SSH in?
- Feel the pain of manual provisioning — this motivates Phase 5

---

## Phase 5: Infrastructure as Code

**Goal:** Codify everything you did manually in Phase 4.

### Concepts First

- Declarative vs imperative infrastructure
- State management and drift detection
- Idempotency
- Immutable infrastructure

### Terraform (Start Here)

- HCL syntax, providers, resources, data sources
- State files and remote backends (S3 + DynamoDB locking)
- Modules for reusability
- `plan` → `apply` workflow
- Import existing resources

### Also Worth Knowing

- **Pulumi** — IaC using real programming languages (Python, Go, TypeScript)
- **Ansible** — configuration management, complementary to Terraform
    - Terraform provisions infrastructure, Ansible configures it

### Practice

- Recreate your Phase 4 cloud deployment entirely in Terraform
- Tear it down, bring it back up, change a parameter — feel the power

**IaC after cloud is intentional.** You need to understand what you're provisioning before you codify it.

---

## Phase 6: Kubernetes

**Goal:** Orchestrate containers at scale. Separate from basic Docker because it's its own domain.

### Core Concepts (the 20% that covers 80%)

- Pods, Deployments, ReplicaSets
- Services (ClusterIP, NodePort, LoadBalancer)
- ConfigMaps and Secrets
- Ingress and Ingress Controllers
- Namespaces
- Resource requests and limits
- Health checks (liveness, readiness probes)

### Practical Skills

- `kubectl` fluency: get, describe, logs, exec, port-forward
- Writing and debugging YAML manifests
- Helm charts for packaging
- Understanding how networking works inside a cluster

### Deployment & GitOps

- ArgoCD (already have exposure from work)
- GitOps workflow: repo as source of truth → auto-sync to cluster

### What to Skip Initially

- Custom operators, CRDs
- Service mesh (Istio/Linkerd) — learn later
- Advanced scheduling, taints/tolerations — learn when you need them

---

## Phase 7: Observability

**Goal:** Tie everything together. If you can't observe it, you can't operate it.

### Three Pillars

- **Metrics:** Prometheus (collection), Grafana (visualization)
- **Logs:** Structured logging, log aggregation (ELK, Loki, Splunk)
- **Traces:** Distributed tracing with OpenTelemetry

### Practical Skills

- Instrument your app with Prometheus client libraries
- Build Grafana dashboards with meaningful alerts
- Set up alerting rules (avoid alert fatigue — alert on symptoms, not causes)
- Understand SLIs, SLOs, SLAs
- Error budgets

### Build On Existing Knowledge

- Already doing structured JSON logging for Splunk
- Already working with Kafka consumers and dead letter queues
- Layer in metrics and tracing on top of what you know

---

## Principles for the Entire Journey

1. **Build, don't just study.** Every phase should produce a working project.
2. **Time box the foundations.** Resist tutorial hell. Move into building early.
3. **Learn concepts, then tools.** Tools change. Concepts transfer.
4. **Stack each phase.** The Django app you containerize in Phase 2 should flow through CI/CD in Phase 3, deploy to cloud in Phase 4, get codified in Phase 5, orchestrated in Phase 6, and observed in Phase 7.
5. **One continuous project > seven separate ones.** The compounding effect is massive.
