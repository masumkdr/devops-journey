# Week 1, Day 5 — Container Networking

My study notes from Day 5 of the AI Agent Infrastructure course.

---

## The big idea: containers reach each other by NAME

On a custom Docker network, containers find each other using their **container name**
as a hostname — no IP addresses needed. Docker runs an **embedded DNS server** that
automatically registers every container's name and resolves it to that container's
current IP.

Example: a container named `db` can be reached by any other container on the same
network at the hostname `db`. A connection string is just `postgres://db:5432`.

---

## How Docker networking works

- A Docker network is a **virtual network** (a virtual bridge/switch). Each container
  attached to it gets an IP on that network's subnet.
- Containers on the **same** network can reach each other.
- Containers on **different** networks are **isolated** — they cannot reach each other
  (different, isolated subnets), unless explicitly connected to a shared network.

### The critical gotcha: default bridge vs custom bridge
- **Default bridge** (what containers join if you don't specify a network):
  does **NOT** provide name-based DNS. `ping <name>` fails here.
- **Custom bridge** (one I create with `docker network create`):
  **DOES** provide name-based DNS. This is the one to use for multi-container apps.
- Rule: whenever containers need to talk to each other, create a custom network.

### Network types (the three that matter now)
- **bridge** — default kind; containers on one host talk to each other.
- **host** — container shares the host's network directly (no isolation).
- **none** — no networking at all (full isolation).

---

## Commands
```bash
docker network create myapp-net      # create a custom bridge network
docker network ls                    # list networks
docker network inspect myapp-net     # see subnet, connected containers, etc.
docker run --network myapp-net ...   # attach a container to a network
docker network connect <net> <ctr>   # attach an existing container to a network
docker network rm myapp-net          # remove a network
```

---

## Experiments I ran

**1. Created and inspected a network** — `docker network inspect` showed the subnet
Docker assigned.

**2. Name resolution on a custom network** — put a `db` (nginx) container and a
`client` (ubuntu) container on `myapp-net`. From inside client:
`ping db` resolved to 172.18.0.2 and `curl http://db` returned nginx's page.
The name resolved via Docker's embedded DNS — I never used an IP.

**3. Default bridge has no name DNS** — put two containers on the default bridge (no
`--network` flag). `ping db2` **failed** to resolve. Confirms name resolution only
works on custom networks.

---

## Why name-based connection is robust (not just convenient)

Container IPs are **dynamic** — a container may get a different IP each time it
restarts. If an app hardcoded an IP like `172.18.0.2`, it would break the moment the
target container restarts with a new IP. The **name** always resolves to wherever the
container currently is, so name-based connection survives restarts, redeploys, and
scaling. This ties back to the core Docker theme: containers are disposable ("cattle,
not pets") — names, volumes, and networks give you stable systems built from
temporary parts.

---

## What clicked for me today
- On a custom network, containers talk by name; Docker's embedded DNS does the lookup.
- Default bridge = NO name DNS; custom bridge = name DNS. Always use a custom network
  for multi-container apps.
- Name > IP because IPs are dynamic and change on restart; the name always resolves.
- Same network = can talk; different networks = isolated.
- For a Node API + Postgres: create a custom network, attach both, connect via
  `postgres://db:5432` using the container name.

---

## Quiz result
Day 5 quiz: passed after a re-check. Corrections locked in:
- The **embedded DNS server** is what resolves names.
- Default bridge does **NOT** do name DNS; custom bridge does.
- Robustness reason = IPs are dynamic; names survive restarts.
- Different networks are isolated subnets — no cross-network access by name.

**Next: Day 6 — Week 1 mini-project: a multi-container app (API + database) wired
together on a custom network.**
