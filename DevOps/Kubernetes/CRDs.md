CRDs extend the Kubernetes API by letting you define your own resource types beyond the built-in ones (Pods, Services, Deployments, etc.). Once you create a CRD, users can create and manage instances of that custom resource using `kubectl` just like any native resource.

**Why they matter:** They're the foundation of the Kubernetes operator pattern. Instead of managing external systems with scripts or manual processes, you define your desired state as a custom resource and let a controller reconcile it.

**How they work:**

A CRD is itself a Kubernetes resource (of kind `CustomResourceDefinition`) that you apply to the cluster. It tells the API server: "there's a new resource type with this name, this schema, and these properties." The API server then dynamically creates RESTful endpoints for CRUD operations on that resource.

**Key components of a CRD spec:**

- **group** — the API group (e.g., `stable.example.com`)
- **names** — kind, plural, singular, shortNames (e.g., kind: `CronTab`, plural: `crontabs`)
- **scope** — `Namespaced` or `Cluster`
- **versions** — one or more versions with an OpenAPI v3 schema for validation
- **additionalPrinterColumns** — custom columns shown in `kubectl get` output

**Example flow:**

1. Define and apply a CRD (e.g., `crontabs.stable.example.com`)
2. Now you can `kubectl create` instances of kind `CronTab`
3. These are stored in etcd like any other resource
4. A custom controller (operator) watches for CronTab events and takes action

**CRDs vs. Aggregated API Servers:**

- CRDs are simpler — no additional infrastructure needed, just a YAML manifest
- Aggregated APIs give you more control (custom storage backends, custom auth, subresources beyond `/status` and `/scale`) but require running a separate API server

**Validation:**

- CRDs support structural schemas (OpenAPI v3) to validate fields, types, required properties, enums, etc.
- You can also use **validation rules** (CEL expressions in newer K8s versions) for more complex cross-field validation without needing a webhook

**Versioning & Conversion:**

- CRDs can serve multiple versions simultaneously
- A **conversion webhook** handles translating between versions (e.g., v1alpha1 → v1beta1)
- One version is marked as the **storage version** (what's persisted in etcd)

**Subresources:**

- `/status` — separates spec updates from status updates (different RBAC, no optimistic locking conflicts between user and controller)
- `/scale` — enables `kubectl scale` and HPA integration

**Common patterns:**

- **Operators** — a CRD + a controller that manages the lifecycle of some system (databases, message queues, certificates, etc.)
- **Composition** — tools like Crossplane use CRDs to represent cloud infrastructure as Kubernetes resources
- **Configuration** — Istio's VirtualService, Gateway; cert-manager's Certificate, Issuer

**Things to be aware of:**

- CRDs are stored in etcd, so they're subject to the same size limits (~1.5MB per object)
- No built-in garbage collection between custom resources unless you set owner references
- Schema changes need care — removing fields can break existing resources
- Finalizers on custom resources can block deletion if the controller is down
- CRDs don't have built-in admission control — use ValidatingAdmissionWebhooks or ValidatingAdmissionPolicies (CEL-based, newer) for complex validation