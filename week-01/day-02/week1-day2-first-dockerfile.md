# Week 1, Day 2 — My First Dockerfile

My study notes from Day 2 of the AI Agent Infrastructure course.

---

## What I built today

I took a small Express app (listening on port 3000), wrote my first Dockerfile,
built it into an image, and ran it as a container that I could reach from my browser.

My Dockerfile:
```dockerfile
FROM node:24-slim
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```
(Note for later: for production, an LTS Node version like 20 or 22 is usually
preferred over the newest release, because LTS gets long-term security patches.)

---

## Dockerfile instructions — what each line does

| Instruction | What it does | When it runs |
|-------------|-------------|--------------|
| `FROM node:24-slim` | Picks the base image to build on top of | — |
| `WORKDIR /app` | Sets the working folder **inside the image** (like `cd`, and creates it) | — |
| `COPY package*.json ./` | Copies dependency files into the image first | build |
| `RUN npm install` | Runs a command at **build time**; bakes dependencies into the image | build |
| `COPY . .` | Copies the rest of my source code into the image | build |
| `EXPOSE 3000` | Documents which port the app listens on (metadata only) | — |
| `CMD ["node", "index.js"]` | The command that runs when a **container starts** (becomes PID 1) | run |

### RUN vs CMD (the key timing distinction)
- `RUN` executes during `docker build` — it changes the image (e.g. installs packages).
- `CMD` executes during `docker run` — it starts the app inside the container.
- `RUN npm install` happens once at build. `CMD` happens every time I start a container.

---

## Build and run commands

```bash
docker build -t my-first-app .
docker run -d --name app1 -p 3000:3000 my-first-app
curl http://localhost:3000
```

### `docker build -t my-first-app .`
- `-t my-first-app` = **tag**, the human-readable name for the image (instead of a cryptic ID).
- `.` = the **build context** — the folder on MY machine that Docker reads files from
  during the build. This is what `COPY . .` copies from.
  - IMPORTANT: the `.` is NOT the working directory inside the image.
    Build context = my host's files. WORKDIR = the image's internal folder. Two different worlds.
  - If I passed `/home/test/myapp/` instead of `.`, Docker would build from that folder.

---

## Port mapping: EXPOSE vs -p

This was the biggest lesson of the day.

- `EXPOSE 3000` does **NOT** open or publish a port. It is pure documentation — a sticky note.
- `-p HOST:CONTAINER` is what actually connects a port on my machine to a port inside
  the container. It works like **port forwarding** between host and container.
- A container lives in its own network namespace (Day 1) — port 3000 inside the container
  is not the same as port 3000 on my laptop until `-p` bridges them.

### The rule
`-p HOST:CONTAINER`
- The **container side (right)** must match the port the app actually listens on inside
  (3000, from `index.js`).
- The **host side (left)** is my free choice — any free port.

Examples for an app listening on 3000 inside:
- `-p 3000:3000` → reach it at localhost:3000 ✓
- `-p 8080:3000` → reach it at localhost:8080 ✓
- `-p 4000:4000` → FAILS ✗ — nothing listens on 4000 inside the container
- Without `-p` at all → app runs but is unreachable from the browser (sealed room)

To make `4000:4000` work, I'd have to change the app to `app.listen(4000)`, rebuild,
and update `EXPOSE 4000` for honesty.

---

## Problem I hit and fixed

**`permission denied ... /var/run/docker.sock`** when running `docker build`.
- Cause: my user's `docker` group membership wasn't active in that terminal session yet.
- Fix: `newgrp docker` (activates the group in the current shell), or log out/in.
- Lesson: don't work around it with `sudo docker ...` — that creates root-owned files
  and causes permission headaches later.

---

## What clicked for me today
- `EXPOSE` is documentation; `-p` is the real port forwarding. Two different things.
- `-p` follows HOST:CONTAINER, and the container side must match what the app listens on.
- Build context (`.`) = the host files Docker builds from; WORKDIR = the folder inside the image.
- `RUN` = build time, `CMD` = run time.

---

## Quiz result
Day 2 quiz: passed (4/5). Correction to remember: the `.` in `docker build ... .`
is the **build context** (host files), NOT the image's working directory.

**Next: Day 3 — layers and caching (why we copied package.json first).**
