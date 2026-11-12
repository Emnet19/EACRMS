Full-Stack CI/CD Syllabus

Day 1: Git Mastery, Local Full-Stack Dockerization & Orchestration
Focus: Containerizing a multi-container architecture and driving local workflows deterministically.
Core Theoretical Concepts
Multi-Container Architectures: Isolation of concerns across independent Frontend, Backend, and Database tiers.
Docker Compose: Orchestrating complex environments locally via multi-service declaration syntax.
Networking & Data Isolation: Designing internal bridge networks for inter-container routing using service alignment, and managing persistent container state via named storage volumes.
Mandatory Skills Tracker
Dockerfiles • docker-compose.yml • Named Volumes • Bridge Networks • Branch Protection
Hands-on Deliverable: Interns must launch their entire full-stack app locally using a single execution of 'sudo docker compose up'. They must demonstrate that runtime transactions successfully persist inside the database volume even after tearing down and destroying all active containers.

Day 2: Docker Hub, Registry Management & Cloud Pulls
Focus: Migrating local artifacts to centralized public/private registries and validating independent host pull operations.
Core Theoretical Concepts
Image Tagging Topologies: Best practices comparing static semantic versioning against deterministic, pipeline-generated unique identifiers like Git Commit SHAs.
Registry Lifecycle Operations: Authenticated channel handling, remote workspace naming strategies, and securing container execution parameters.
Mandatory Skills Tracker
docker login • docker tag • docker push • docker pull • Access Tokens
Hands-on Deliverable: Interns must securely push their compiled frontend and backend images into Docker Hub. They must then execute an isolated 'docker pull' on an external cloud instance or alternative virtual container host to recreate the application environment without the presence of local source files.

Day 3: The CI Pipeline Bible (Build, Test, Push & Merge Automation)
Focus: Formulating an ironclad Continuous Integration mechanism that strictly enforces software verification steps prior to branch convergence.
Core Theoretical Concepts
Automated Gating Mechanics: Structuring triggered pipeline configurations inside GitHub Actions to decouple integration hooks from human sign-off.
Testing Execution Isolation: Running testing frameworks safely within short-lived, automated virtual environments.
Artifact Promotion Workflow: Linking authenticated registry updates to successful validation suites.
Mandatory Skills Tracker
GitHub Actions • YAML Workflow Definition • Secrets Injection • Layer Caching
Hands-on Deliverable: Establish a live automated CI rule. Opening a Pull Request to the production branch must autonomously trigger linting and execution of the integrated test suites. Upon passing and merging, the pipeline must seamlessly handle compiling and pushing updated production images directly to Docker Hub without intervention.

Day 4: Continuous Deployment (CD) to Cloud Platforms
Focus: Closing the delivery cycle by bridging verified artifacts directly into a live, user-facing production space.
Core Theoretical Concepts
Automated Delivery Channels: Integrating webhook hooks or remote provider command line tool chains to execute container initialization.
State & Runtime Integrity: Keeping application environment variables entirely distinct from source control histories.
Zero-Downtime Deployment: Mitigating system disruption using traffic-switching deployment paradigms.
Mandatory Skills Tracker
Deploy Hooks • Cloud Infrastructure Management • Log Auditing • Health Checks
Hands-on Deliverable: Wire up automated web hooks or configuration flags connecting the Docker Hub registry or GitHub Actions output to cloud compute environments (such as Render or Vercel). Merging code must produce a fully automated rollout that terminates at a live, secure public domain address.

Day 5: Production Architecture (Nginx, Load Balancing & Monitoring)
Focus: Hardening systems to withstand scale demands using load balancers and establishing deep operational visibility.
Core Theoretical Concepts
Nginx Reverse Proxy: Acting as an ingress layer for traffic routing, processing, and caching optimization.
Load Balancing Topologies: Distributing state processing evenly across multiple parallel application container targets.
Time-Series Observability: Configuring Prometheus data scraping endpoints and styling actionable analytical tracking dashboards in Grafana.
Mandatory Skills Tracker
nginx.conf Upstream • Reverse Proxying • Prometheus Metrics • Grafana Dashboards
Hands-on Deliverable: Interns must construct a proxy layer utilizing Nginx to distribute inbound requests between two replicas of their backend container instances. Concurrently, they must spin up an observation container stack monitoring engine resource allocations and live server metrics in a clean Grafana visual layout.

Skills Evolution Matrix
Timeline	Target Tier	Tooling Context	Core Structural Principle
Day 1	Beginning	Docker, Compose, Git	Isolate local environment configurations completely inside micro-containers.
Day 2	Lower-Intermediate	Docker Hub, Remote CLI	Decouple production-ready application artifacts from base code state.
Day 3	Intermediate	GitHub Actions, Testing Suite	Enforce deterministic quality gates; forbid manual unchecked merging.
Day 4	Upper-Intermediate	Render, Vercel, Production CD	Map successful development integrations straight to system rollouts.
Day 5	Advanced	Nginx, Prometheus, Grafana	Engineer multi-instance traffic resilience and system transparency.
