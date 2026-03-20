A YAML (or JSON) file that describes a **desired state** you want in your cluster. 
- You hand it to the API server (via `kubectl apply`, ArgoCD sync, etc.) and Kubernetes continuously works to make reality match what you declared. **Declarative Derived State**
- Think of it like a *contract*: 
	- "I want 3 replicas of this container, with this much memory, exposed on this port."
	- Kubernetes reads the contract and makes it happen. 
	- If a Pod dies, the controller sees the actual state drifted from desired state and reconciles.
## Core building blocks
Every manifest describes a **resource** — here are the ones that cover ~90% of what you'll interact with:
- **Workloads** (things that run your code): Deployments, StatefulSets, DaemonSets, Jobs, CronJobs. These all create and manage Pods.
- **Networking** (things that route traffic): Services (ClusterIP, NodePort, LoadBalancer), Ingress, NetworkPolicies.
- **Configuration** (things that feed data to your code): ConfigMaps, Secrets.
- **Storage** (things that persist data): PersistentVolumeClaims, PersistentVolumes, StorageClasses.
- **Access control**: ServiceAccounts, Roles, RoleBindings, ClusterRoles, ClusterRoleBindings.
## Skeleton (every K8s manifest follows)
```
apiVersion: <group/version>
kind: <resource type>
metadata:
  name: ...
  namespace: ...
  labels: ...
spec:
  <the desired state details>
```