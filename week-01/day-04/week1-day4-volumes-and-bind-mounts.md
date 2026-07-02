# Week 1, Day 4 — Volumes & Bind Mounts

My study notes from Day 4 of the AI Agent Infrastructure course.

---

## The problem: containers are disposable

By default, anything a container writes goes into its own thin **writable layer**.
When the container is **removed** (`docker rm`), that layer is destroyed and the data
is gone forever. Fine for a stateless web server; catastrophic for a database.

(Two related but distinct Day-1 ideas:
- Stopping a container ends its PID 1 process.
- Removing a container deletes its writable layer → the data inside disappears.)

So we need storage that lives *outside* the container. There are two options.

---

## The three places data can live

| Option | Managed by | Location | Survives removal? | Use it for |
|--------|-----------|----------|-------------------|------------|
| Inside container | nobody (default) | the container's writable layer | ❌ NO | nothing important |
| **Named volume** | Docker | `/var/lib/docker/volumes/<name>/_data` | ✅ YES | databases, app data |
| **Bind mount** | me (I choose the path) | any host folder I point to | ✅ YES | live-editing code in dev |

### Volume vs bind mount — the key distinction
- **Named volume**: Docker creates and manages it. I refer to it by a **name**
  (e.g. `mydata`) and don't care where it physically sits.
- **Bind mount**: I map a **specific host folder** into the container by giving its **path**.
- Docker tells them apart by the left side of `-v`: a name = volume, a path (starts
  with `/` or `~`) = bind mount.

---

## Syntax (the `-v` flag — like `-p` but for filesystem paths)

```bash
# Named volume  (left side = a NAME)
-v mydata:/path/in/container

# Bind mount    (left side = a HOST PATH)
-v /home/me/project:/path/in/container
```

Format is `SOURCE:TARGET` — source on the host side, target inside the container.

---

## Experiments I ran

**1. Data dies inside a container**
Wrote `/data.txt` inside a container, removed the container, started a new one →
file was gone. Confirms the writable layer dies with the container.

**2. Named volume persists**
```bash
docker volume create mydata
docker run -it --name vtest -v mydata:/stuff ubuntu bash   # wrote /stuff/file.txt
docker rm vtest
docker run -it --name vtest2 -v mydata:/stuff ubuntu bash  # file still there ✓
```
Data survived container deletion because it lived in the volume, not the container.

**3. Bind mount — two-way live editing**
```bash
docker run -it -v ~/bindtest:/mnt/host ubuntu bash
```
Files written inside the container appeared on my host, and host files appeared inside
the container — instantly, both directions. This is the basis of a live dev loop:
edit code in my editor, changes appear in the running container with no rebuild.

---

## Useful volume commands
```bash
docker volume ls               # list all volumes
docker volume inspect mydata   # details incl. the real host path
docker volume rm mydata        # remove a volume (only if no container uses it)
docker volume prune            # remove ALL unused volumes — use with care!
```

**Caution:** volumes are NOT deleted when you `docker rm` a container. This is
protective (your DB survives accidental container removal) but means orphaned volumes
pile up over time and eat disk. Prune carefully — "unused" might include data you need.

---

## What clicked for me today
- Never store important data in the container itself — it dies on removal.
- Volume = Docker-managed, referenced by name → for persistent app/DB data.
- Bind mount = a host folder I choose → for live-editing code during development.
- Both survive container removal; they differ in who manages them and what they're for.
- Postgres data → named volume (it's the app's data, not something I hand-edit).
- Source code in dev → bind mount (edits appear instantly, no rebuild).

---

## Quiz result
Day 4 quiz: passed 5/5. Storage model fully locked in, including the reasoning
behind choosing volumes for databases and bind mounts for dev code.

**Next: Day 5 — container networking (how containers talk to each other).**
