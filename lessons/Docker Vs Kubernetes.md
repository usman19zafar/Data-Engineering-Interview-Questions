Assumptions made by Docker / Docker Compose

Docker Compose is designed for single-machine orchestration, mainly for development, testing, and local environments.

1️⃣ Networking

Each container is attached to a default bridge network (unless configured)

Containers can resolve each other by service name within the same compose project

localhost refers only to the container itself

Port mappings are optional; Docker does not assume host ports are needed

Containers assume network latency is negligible (single machine)

2️⃣ Storage / Filesystem

Containers share volumes explicitly; otherwise, filesystem is ephemeral

No assumption about distributed filesystem

Mount paths are consistent with the host OS, but no multi-host guarantees

3️⃣ Startup order & readiness

depends_on can define order, but not readiness

Docker assumes the container will start successfully; it does not check application readiness

Health checks are optional; if missing, Docker assumes the service is “ready” once started

4️⃣ Statefulness

Containers can be stateful or stateless, but Docker doesn’t enforce recovery

No automatic replication or failover; single instance is assumed

5️⃣ Scheduling & resources

Assumes all containers run on the same host

CPU / memory limits are optional

Docker does not assume multi-host resource management

6️⃣ Image versions

If unspecified, uses latest; Docker assumes user responsibility for stability

Docker assumes images are compatible with the host OS

7️⃣ Secrets / configuration

Environment variables / .env files are optional

Docker assumes configuration is trusted and available locally

8️⃣ Scale

Scaling services (docker-compose up --scale) is supported, but networking & persistence are not automatically handled

Assumes local development, not global distribution

9️⃣ Security

Docker does not enforce inter-container security by default

Assumes a trusted developer environment

B. Assumptions made by Kubernetes

Kubernetes is designed for multi-host orchestration, production workloads, and distributed systems.

1️⃣ Networking

Containers (pods) are ephemeral; IPs change constantly

localhost refers only to the pod itself

Services must be accessed via service discovery / DNS

Pods can communicate across nodes via cluster network

Network latency and partial failures are normal

Ports inside pods are ephemeral; host port mapping is discouraged in production

2️⃣ Storage / Filesystem

Pods are ephemeral; local filesystem is not persistent

Persistent storage must be via Persistent Volumes (PV) / Persistent Volume Claims (PVC)

Assumes distributed, dynamic storage (Cloud storage, NFS, etc.)

3️⃣ Startup order & readiness

Pods can start in any order

No built-in startup order (depends_on does not exist)

Readiness and liveness probes are required to mark pod readiness

Kubernetes assumes applications can fail and restart gracefully

4️⃣ Statefulness

Pods are ephemeral and replaceable

StatefulSets are used for apps that need stable identity (Kafka, databases)

Assumes failure/restart is normal; no persistent state inside pod

5️⃣ Scheduling & resources

Kubernetes schedules pods on any available node

Assumes multi-node cluster, possible resource contention

CPU/memory limits are enforced by the scheduler

Can preempt or evict pods

6️⃣ Image versions

Assumes immutability of container images for predictable deployment

Rolling updates are atomic; Kubernetes assumes versioned deployments

7️⃣ Secrets / configuration

Assumes configuration is externalized (ConfigMaps, Secrets)

Environment variables and mounted secrets are ephemeral; pods are stateless

8️⃣ Scale

Horizontal and vertical scaling are assumed normal

Pods can appear/disappear frequently; the system tolerates scaling events

Load balancing and service discovery are integral

9️⃣ Security

Zero-trust model is assumed

Pods are isolated via namespaces, network policies

No assumption that containers are fully trusted

10️⃣ Failure / resilience

Node failures are normal

Pods, nodes, and containers can die anytime

Assumes self-healing is handled (restart policies, ReplicaSets, StatefulSets)

Rolling updates, rollbacks, and health-based replacement are expected

C. Key difference: Docker vs Kubernetes assumptions
Aspect	Docker	Kubernetes
Host	Single machine	Multi-node cluster
IP / networking	Static per container	Dynamic per pod, uses service discovery
Localhost	Mostly works in dev	Localhost always isolated
Startup	depends_on optional	No startup order, readiness probes required
State	Optional persistence	Stateless by default, use PVs for persistence
Scaling	Optional, limited	Normal, dynamic, auto-scaled
Failure	Assumes success	Assumes pods/nodes fail anytime
Security	Trusted local environment	Zero-trust, isolated by default
Versioning	Latest images common	Immutable, pinned, production-grade
CI/CD assumption	Local environment forgiving	Strict, deterministic, clean slate

✅ TL;DR mental model

Docker = sandbox for local orchestration

Kubernetes = distributed, production-grade orchestrator that enforces assumptions Docker hides

Everything that “works on your laptop” will be tested against these stricter assumptions in CI/CD or Kubernetes — which is exactly why enterprise pipelines care deeply about Docker/Kubernetes discipline.
