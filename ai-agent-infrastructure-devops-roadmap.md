# Roadmap: DevOps Engineer for AI Agent Infrastructure

**Target role:** The engineer who takes AI agents from "works on my laptop" to production — deployed, secured, observed, cost-controlled, and scaled. This role sits at the intersection of platform engineering and AI engineering, and very few people can do both. Your background (networking, server admin, full-stack Node/TS, Claude Code, MCP, APIs) is almost perfectly shaped for it.

**Timeline:** 12 months, ~15–20 hours/week. Compress to 8–9 months if you can do more.

---

## What this role actually does (so you can aim at it)

- Deploys and scales LLM-powered services and agent systems (APIs, MCP servers, background agents)
- Builds LLM gateways: routing, rate limiting, failover between providers, cost controls
- Secures agent execution: sandboxing agent-generated code, secrets management, least-privilege tool access
- Builds observability for agents: token/cost tracking, tracing, latency, eval metrics
- Builds CI/CD for AI systems: prompt versioning, eval gates in pipelines, canary releases for prompts/models
- Manages the supporting infrastructure: queues, caches, vector databases, state stores

Job titles to search for: **Platform Engineer (AI), AI Infrastructure Engineer, LLMOps Engineer, DevOps Engineer – AI/ML, Agent Infrastructure Engineer, MLOps Engineer.**

---

## Phase 1 — DevOps core (Months 1–2)

You have Linux/networking/server experience, so this phase is about modern tooling, not fundamentals.

**Learn:**
- Docker deeply: multi-stage builds, image optimization, networking, volumes, docker-compose, container security basics (non-root users, minimal base images)
- CI/CD with GitHub Actions: build → test → scan → deploy pipelines, secrets, matrix builds, reusable workflows
- Bash + scripting automation (refresh)
- Nginx / reverse proxies / TLS (refresh from your server admin days)

**Do:**
- Containerize one of your existing Node.js/React projects properly (small image, healthchecks, non-root)
- Build a full GitHub Actions pipeline for it: lint → test → build image → push to registry → deploy

**Milestone:** A repo with a production-grade Dockerfile and CI pipeline you can show.

---

## Phase 2 — Cloud + Infrastructure as Code (Months 2–4)

**Learn:**
- AWS core: VPC, EC2, ECS/Fargate, S3, RDS, IAM, CloudWatch, Lambda, API Gateway, Bedrock (AWS's LLM service — relevant to your niche)
- Terraform: providers, state, modules, workspaces, remote state in S3

**Certify:**
- **AWS Certified Solutions Architect – Associate** ($150) — Month 3
- **HashiCorp Terraform Associate** (~$70) — Month 4

**Do:**
- Rebuild your Phase 1 project's infrastructure entirely in Terraform: VPC, ECS service, load balancer, RDS, secrets in SSM/Secrets Manager
- Deploy via the CI pipeline (GitOps-lite: merge to main = deploy)

**Milestone:** `terraform apply` stands up your whole stack from zero. This repo becomes portfolio piece #1.

---

## Phase 3 — Kubernetes + Observability (Months 4–7)

**Learn:**
- Kubernetes: pods, deployments, services, ingress, ConfigMaps/Secrets, RBAC, HPA autoscaling, network policies
- Helm charts
- GitOps with ArgoCD
- Observability: Prometheus + Grafana, structured logging (Loki), OpenTelemetry tracing

**Certify:**
- **CKA – Certified Kubernetes Administrator** — the hands-on terminal exam that carries the most weight for degree-less candidates. List price ~$445; Linux Foundation runs 40–50% sales several times a year (Black Friday, New Year) — wait for one. Practice with killer.sh (two free sessions included with the exam).

**Do:**
- Run a local cluster (kind/k3s) then a small cloud cluster (EKS or a cheap alternative like k3s on a VPS to control cost)
- Deploy your project to K8s with Helm + ArgoCD, wire up Prometheus/Grafana dashboards and alerts

**Milestone:** CKA passed + a GitOps-managed cluster with real dashboards.

---

## Phase 4 — AI agent infrastructure specialization (Months 6–9, overlaps Phase 3)

This is where you stop being "another DevOps engineer" and become the niche. You already know MCP, Claude Code, and the APIs — now productionize that knowledge.

**Learn:**
1. **LLM gateway patterns** — LiteLLM or a self-built gateway: provider routing, retries/failover, rate limiting, per-team API keys, cost caps, response caching
2. **Production MCP servers** — take MCP beyond localhost: remote MCP over HTTP, OAuth/auth, containerized deployment, horizontal scaling, versioning
3. **Agent sandboxing & security** — running agent-generated code safely: container isolation, resource limits, network egress controls, gVisor/Firecracker concepts, least-privilege tool permissions, prompt injection defenses at the infra layer
4. **State & memory infrastructure** — Redis for session state, Postgres + pgvector for embeddings/RAG, message queues (SQS/RabbitMQ) for async agent tasks
5. **Agent observability** — Langfuse (open source, self-hostable — perfect portfolio material) or LangSmith: tracing agent runs, token/cost per request, latency breakdowns; OpenTelemetry GenAI conventions
6. **CI/CD for AI systems** — prompt/config versioning, automated eval suites as pipeline gates, canary rollouts for prompt changes, regression testing agents

**Certify (lightweight, for resume keywords only):**
- AWS Certified AI Practitioner ($100) — optional, cheap, quick given your knowledge

**Do — the three flagship portfolio projects:**

**Project A: Multi-tenant MCP platform.** MCP servers deployed on Kubernetes with authentication, per-tenant isolation, autoscaling, and Grafana dashboards. Publish the Helm chart and Terraform modules. Almost nobody has this in their portfolio yet.

**Project B: AI-augmented CI/CD pipeline.** A GitHub Actions pipeline where Claude (via API) reviews PRs, an eval suite gates prompt changes, and release notes are auto-generated. Infrastructure fully in Terraform.

**Project C: Secure agent execution sandbox.** A service that runs agent-generated code in locked-down containers with CPU/memory/network limits and full audit logging. Write up the threat model — the write-up matters as much as the code.

**Milestone:** Three public repos with READMEs, architecture diagrams, and a short blog post each.

---

## Phase 5 — Positioning & landing work (Months 9–12)

**Portfolio & presence:**
- Pin the three flagship projects on GitHub; write clear READMEs with architecture diagrams
- Publish at least one open-source MCP server or contribute to an existing AI infra project (Langfuse, LiteLLM, MCP ecosystem) — merged PRs are credibility you can't buy
- Write 4–6 short technical posts (LinkedIn / dev.to / your own blog): "Deploying MCP servers on Kubernetes," "Cost observability for Claude agents," etc. These posts get you found.
- LinkedIn headline: "Platform Engineer — AI Agent Infrastructure | AWS SAA · Terraform · CKA" (certs in the headline matter when you have no degree)

**Where to find work (from Bangladesh):**
- **Upwork/Contra:** AI agent development is one of the fastest-growing categories; infrastructure-capable agent developers are rare. Start with smaller MCP/agent-deployment gigs to build reviews, then raise rates.
- **Remote job boards:** Wellfound (startups), RemoteOK, We Work Remotely, LinkedIn remote filters. AI startups hire globally and care about proof-of-work over degrees.
- **Local:** Bangladeshi banks, telcos, and startups adopting AI need exactly this profile; BASIS/Hi-Tech Park incentives apply to IT freelancers/exporters.
- Set up **Payoneer** early for international payments.

**Interview prep:**
- Be ready to whiteboard: "Design an infrastructure for 1,000 concurrent AI agents" — gateway, queue, sandbox, state store, observability. Your projects ARE the answer.

---

## Certification summary

| # | Certification | When | Cost (approx.) | Why |
|---|---------------|------|------|-----|
| 1 | AWS Solutions Architect – Associate | Month 3 | $150 | Most-requested cloud cert; resume filter pass |
| 2 | HashiCorp Terraform Associate | Month 4 | $70 | IaC proof; quick win |
| 3 | CKA (Kubernetes Administrator) | Month 7–8 | $250–445 (buy on sale) | Hands-on exam; the strongest signal without a degree |
| 4 | AWS AI Practitioner (optional) | Month 9 | $100 | Cheap keyword match for AI roles |

Total: roughly **$570–770** (~৳70,000–95,000) over 12 months. Each cert should pay for itself quickly at remote rates.

---

## Weekly rhythm (suggested)

- 8–10 hrs: current phase learning (course + labs)
- 5–7 hrs: building the current portfolio project
- 2–3 hrs: writing, LinkedIn, community (from Phase 4 onward)

## Rules that keep this on track

1. Never study without building — every concept goes into a repo the same week.
2. Buy exams on discount; never pay CKA list price.
3. Ship projects at 80% polish and write about them — done and public beats perfect and private.
4. Your AI/MCP knowledge is the differentiator; the DevOps certs are the trust layer. Lead with both.
