# Course Plan: AI Agent Infrastructure Engineer

**Student:** Full-stack developer (Node.js/TS/React), ex-networking & server admin, strong Claude Code/MCP/API knowledge, learning DevOps. No degree — portfolio + certs are the credential.
**Format:** Self-paced with Claude as teacher, reviewer, and quiz-master.
**Load:** ~15–20 hrs/week. Each module ends with a graded checkpoint (Claude reviews your work).

---

## How to study with Claude (the system)

**Setup (do once):**
1. In Claude, create a **Project** called "AI Infra Course" and upload this file plus the roadmap file to its knowledge. Every chat inside that Project will automatically know the plan.
2. Create a GitHub repo `devops-journey` — every week's work gets committed there. Public. This becomes evidence of consistency.

**Daily session ritual (60–120 min):**
1. Start the chat with: "Week X, Day Y — [topic]. Ready." Claude teaches the concept in 15–20 min of reading/discussion.
2. Do the hands-on task on your own machine (this is 70% of the time).
3. Paste your work back (Dockerfile, YAML, commands, errors). Claude reviews it like a senior engineer — you fix, resubmit.
4. End by asking Claude for a 5-question quiz on the day's topic. Below 4/5 = revisit tomorrow before moving on.

**Weekly ritual:**
- Day 6: build the week's mini-project without notes. Paste it to Claude for a code review.
- Day 7: rest, or ask Claude for a mixed quiz covering everything so far (spaced repetition).

**Rules:**
- Never copy-paste a solution you don't understand — ask "why" until you can explain it back.
- Every error message is a lesson: paste it, debug it with Claude, write one line in a `TIL.md` file about what you learned.
- Stuck more than 30 minutes alone → bring it to Claude. Struggling briefly is learning; struggling for hours is waste.

---

## Course overview (12 months → 5 modules)

| Module | Topic | Weeks | Checkpoint |
|--------|-------|-------|------------|
| 1 | Docker & CI/CD mastery | 1–8 | Production-grade containerized app + full pipeline |
| 2 | AWS + Terraform | 9–16 | AWS SAA exam + Terraform Associate exam |
| 3 | Kubernetes + observability | 17–30 | CKA exam + GitOps cluster with dashboards |
| 4 | AI agent infrastructure | 24–38 (overlaps) | 3 flagship projects shipped |
| 5 | Portfolio, writing, job hunt | 39–52 | First paid remote work / role |

---

## MODULE 1 — Docker & CI/CD (Weeks 1–8) — detailed

### Week 1: Docker fundamentals
- **Day 1:** What containers actually are (namespaces, cgroups — your Linux background makes this fast). Install Docker. Run, exec, logs, inspect.
- **Day 2:** Images vs containers. Write your first Dockerfile for a Node.js "hello world" API.
- **Day 3:** Layers and caching. Rebuild your image 5 ways, watch what invalidates cache. `.dockerignore`.
- **Day 4:** Volumes and bind mounts. Persist data; live-reload a dev container.
- **Day 5:** Container networking — bridge networks, port publishing, container-to-container DNS. (You'll enjoy this one.)
- **Day 6 mini-project:** Containerize a real Node.js API of yours.
- **Quiz topics:** layers, cache busting, CMD vs ENTRYPOINT, EXPOSE vs -p.

### Week 2: Production-grade Docker
- Multi-stage builds (TS compile stage → slim runtime stage)
- Image slimming: alpine/distroless, measuring with `docker history`
- Non-root users, read-only filesystems, healthchecks
- Image scanning with Trivy
- **Mini-project:** Get your Week 1 image under 150MB, non-root, with healthcheck, zero critical CVEs.

### Week 3: docker-compose & multi-service apps
- Compose files, service dependencies, env files, profiles
- Add Postgres + Redis to your API via compose
- Networking between services, named volumes, init scripts
- **Mini-project:** Full local stack: API + Postgres + Redis + Nginx reverse proxy, one `docker compose up`.

### Week 4: Git workflows & GitHub Actions basics
- Trunk-based vs GitFlow, conventional commits, branch protection
- Actions anatomy: workflows, jobs, steps, runners, triggers
- First pipeline: lint + test on every PR
- **Mini-project:** CI pipeline on your compose project — lint, typecheck, unit tests, all green badges.

### Week 5: CI deep dive
- Caching node_modules/build artifacts, matrix builds (Node 18/20/22)
- Secrets and environments, OIDC basics
- Building and pushing Docker images to GHCR from CI
- **Mini-project:** Merge to main → image built, scanned (Trivy), pushed to GHCR with git-SHA tags.

### Week 6: CD — actually deploying
- Get a cheap VPS (or free-tier cloud VM)
- Deploy via SSH action or watchtower pull pattern; zero-downtime basics
- TLS with Let's Encrypt/Caddy
- **Mini-project:** Merge to main → live on the internet with HTTPS, automatically.

### Week 7: Pipeline hardening + Claude integration (first AI infra taste)
- Add a job where Claude (via API) reviews PR diffs and comments
- Dependabot, CodeQL, required checks
- **Mini-project:** Your pipeline now has an AI review stage — your first portfolio-worthy AI-infra artifact.

### Week 8: MODULE 1 CHECKPOINT
- Rebuild everything from an empty repo in one day, no notes
- Write the README with an architecture diagram
- Submit to Claude for a full graded review (rubric: image quality, pipeline design, security, docs)
- Write your first LinkedIn/dev.to post: "What I learned shipping a production-grade pipeline"

---

## Module 2 preview (Weeks 9–16)
AWS core services week-by-week (VPC → compute → storage → IAM → serverless), Terraform from zero, then two exam-prep weeks with daily practice tests via Claude. Detailed plan will be generated when you finish Module 1 — pacing depends on how Module 1 goes.

---

## Progress tracker

| Week | Status | Score | Notes |
|------|--------|-------|-------|
| 1 | ⬜ | | |
| 2 | ⬜ | | |
| 3 | ⬜ | | |
| 4 | ⬜ | | |
| 5 | ⬜ | | |
| 6 | ⬜ | | |
| 7 | ⬜ | | |
| 8 | ⬜ | | |

Update this file (or ask Claude to) as you complete each week.
