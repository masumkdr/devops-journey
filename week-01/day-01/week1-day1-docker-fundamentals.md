# Week 1, Day 1 — Docker Fundamentals

My study notes from Day 1 of the AI Agent Infrastructure course.

---

## Core concept: what a container actually is

A **container** is a normal Linux process, isolated by **namespaces** (what it can *see*) and limited by **cgroups** (what it can *use* — CPU, memory), running on a filesystem built from **image layers**. It shares the **host kernel**.

Key insight: nothing "boots" when a container starts. No operating system loads, no kernel starts. The host kernel simply starts one more process with isolation applied. That is why containers start in milliseconds while virtual machines take a minute.

- **Namespaces = the blinders** → own process list, own filesystem, own network. This is why `ps aux` inside my Ubuntu container showed only ~2 processes: it was in its own PID namespace and could not see the host's processes.
- **cgroups = the leash** → limits on CPU, memory, and IO.
- **VM vs container**: A VM = fake hardware + full OS + its own kernel. A container = one process wearing blinders, sharing the host kernel.

### The PID 1 rule
A container lives exactly as long as its **main process** (PID 1 inside the container). Kill that process — for example, type `exit` in a bash container — and the container stops. This explains why `docker run -d nginx` keeps running (nginx does not exit) but `docker run -it ubuntu bash` stops the moment I exit the shell.

---

## The building blocks (four terms)

| Term | What it is | Analogy |
|------|-----------|---------|
| **Dockerfile** | A text file with the recipe of build steps | The recipe |
| **Image** | A frozen, read-only template built from the Dockerfile | The cookie cutter / a class |
| **Container** | The image running live as a process | The cookie / an object |
| **Docker Engine** | The background software that builds images and runs containers | The kitchen |
| **Docker Desktop** | Optional GUI wrapper — **not needed on Linux** | A dashboard I don't use |

The flow: I write a **Dockerfile** → the **Engine** builds an **image** → running the image creates a **container**.

The single most important relationship: **image = template at rest, container = image running.** One image can produce many containers.

---

## Commands and flags I practised

| Command / flag | Meaning | Why I use it |
|----------------|---------|--------------|
| `docker run` | Start a **new** container from an image | Birth of a new container |
| `docker exec` | Run a command **inside an already-running** container | Step into a room that's already occupied |
| `-d` | Detached | Run in the background so the terminal stays free (servers, databases) |
| `--name x` | Name the container | Friendly name instead of a random ID like `elegant_bhaskara` |
| `-i` | Interactive | Keeps input open so my keystrokes reach the container |
| `-t` | Terminal (TTY) | Gives a proper shell prompt; almost always paired as `-it` |
| `docker ps` | List **running** containers | See what's alive right now |
| `docker ps -a` | List **all** containers, including stopped/exited | Find containers that stopped (their main process exited) |
| `docker logs x` | Read a container's output | See what a background container printed; add `-f` to follow live |
| `docker stop x` | Gracefully halt a running container | Politely shut it down |
| `docker rm x` | Delete a stopped container | Remove it from my machine (must stop it first) |

### Cleanup pattern
```bash
docker stop $(docker ps -q)    # stop all running containers
docker rm $(docker ps -aq)     # remove all containers (running + stopped)
```
`docker ps -q` lists only the IDs of running containers; `-aq` lists IDs of all containers including stopped ones.

---

## Experiments I ran and what I observed

1. **`docker run nginx` vs `docker run -d nginx`**
   - Without `-d`: the terminal froze and showed nginx's live logs — I couldn't type until I pressed Ctrl+C (which also stopped the container).
   - With `-d`: I got the container ID printed and my prompt back immediately; nginx kept running in the background.

2. **`docker run -it ubuntu bash`** — dropped me inside an Ubuntu container. `ps aux` showed almost nothing (own PID namespace). Typing `exit` stopped the container (PID 1 died).

3. **`docker exec` proof** — `docker exec web touch /I_WAS_HERE` then `docker exec web ls /` showed the file was still there → both commands ran in the **same** container. `exec` does NOT create a new container; it runs a process inside the existing one.

4. **`docker ps` vs `docker ps -a`** — stopped containers disappeared from `docker ps` but were still visible under `docker ps -a`.

---

## What clicked for me today

- A container is not a mini-VM — it's just a host process wearing namespace blinders, sharing the host kernel.
- `run` = birth of a new container; `exec` = walk into a container that's already running.
- `docker ps` shows running; `docker ps -a` shows all including stopped. If a container vanished from `docker ps`, its main process exited.
- Without `-d` the terminal is held hostage by the container's logs; with `-d` it runs in the background.

---

## Quiz result
Day 1 retake: passed (4.5 / 5). One correction to remember: `docker ps -a` shows **all** containers including **stopped** ones — not just running ones.

**Next: Day 2 — writing my first Dockerfile.**
