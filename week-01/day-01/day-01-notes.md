# Week 1, Day 1 — Docker fundamentals

## Core concepts

**Container** = a normal Linux process, isolated by namespaces (what it can SEE)
and limited by cgroups (what it can USE — CPU, memory), running on a filesystem
built from image layers. It shares the HOST kernel. Nothing boots — it starts in
milliseconds because no OS loads.

- Namespaces = the blinders (own process list, own filesystem, own network)
- cgroups = the leash (CPU/memory limits)
- VM = fake hardware + full OS + own kernel. Container = one process wearing blinders.

**Image vs Container**
- Image = frozen, read-only template (the cookie cutter / the class).
- Container = the image running (the cookie / the object).
- One image → many containers.

**The PID 1 rule**: a container lives exactly as long as its main process.
Kill the main process (e.g. type `exit` in a bash container) → container stops.

## The four terms
- Dockerfile = the recipe (text file of build steps)
- Image = frozen template built from the Dockerfile
- Container = the image running live
- Docker Engine = the background software that builds images and runs containers
- Docker Desktop = optional GUI; NOT needed on Linux (I use the Engine directly)

## Commands & flags
| Command / flag | Meaning | Why |
|---|---|---|
| `docker run` | new container | start a fresh container from an image |
| `docker exec` | step inside | run a command in an ALREADY-running container |
| `-d` | detached | run in background, terminal stays free (for servers/DBs) |
| `--name x` | name it | friendly name instead of random ID |
| `-i` | interactive | keystrokes reach the container |
| `-t` | terminal | proper shell prompt; paired as `-it` |
| `docker ps` | list running | running containers only |
| `docker ps -a` | list all | includes STOPPED/exited containers |
| `docker logs x` | read output | see background container output; `-f` to follow live |
| `docker stop x` | halt | gracefully stop |
| `docker rm x` | delete | remove a stopped container |

## Cleanup pattern
docker stop $(docker ps -q)   # stop all running
docker rm $(docker ps -aq)    # remove all containers

## What clicked for me today
- Without `-d` the terminal freezes and shows live logs; with `-d` it runs in
  background and I get my prompt back.
- `run` = birth of a new container; `exec` = walk into a room already occupied.
- `docker ps` shows running; `docker ps -a` shows all including stopped —
  if a container vanished from `ps`, its main process exited.
