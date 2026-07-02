# Week 1, Day 3 — Layers & Caching

My study notes from Day 3 of the AI Agent Infrastructure course.

---

## The big idea: an image is a stack of layers

A Docker image is structurally a **stack of read-only layers** — one layer per
Dockerfile instruction (`FROM`, `WORKDIR`, `COPY`, `RUN`...), stacked bottom to top.
The final image is all those layers glued together.

"Read-only" is a real property: layers can't be modified after they're built,
which is exactly why they can be safely cached and reused.

```
COPY . .                 <- top    (source code — changes often)
RUN npm install          <-        (dependencies — slow step)
COPY package*.json ./    <-        (dependency list — changes rarely)
WORKDIR /app             <-
FROM node:20-slim        <- bottom (base image — foundation)
```

---

## The caching rule (the whole lesson)

When rebuilding, the **Docker Engine** walks the stack from the bottom and, at each
layer, asks "has anything about this step changed since last time?"
- For `COPY`: it fingerprints the file contents. One byte different = new layer.
- For `RUN`: it checks the command text and whether the layers below changed.

- If nothing changed → reuse the cached layer instantly.
- If a layer changed → rebuild that layer **AND every layer above it**, because each
  layer sits on the one below. A change low in the stack ripples upward.

### The ordering principle
**Stable things at the bottom, volatile things at the top.**
- System packages (change almost never) → lowest
- Language dependencies (`npm install`, change rarely) → middle
- Source code (`COPY . .`, changes constantly) → top

This is why we copy `package*.json` and run `npm install` BEFORE `COPY . .`:
editing source code then only invalidates the top layer, so the slow `npm install`
layer stays cached. The reason is **cache efficiency**, not whether dependencies get
included — with correct ordering, dependencies ARE baked into the image at build time.

---

## Experiments I ran

**1. Rebuild with no changes** → every step showed `CACHED`, finished instantly.

**2. Changed `index.js` (code)** → only `COPY . .` reran. `npm install` was SKIPPED
(stayed cached). Everything below the code layer was reused.

**3. Changed `package.json` (dependency)** → `COPY package*.json`, `RUN npm install`,
and `COPY . .` all reran. Only `FROM` and `WORKDIR` stayed cached. More rebuilding
than experiment 2, because the change hit a lower layer.

### Why experiment 3 rebuilt more than experiment 2
Changing `package.json` invalidated a layer LOWER in the stack, so everything above it
(including the slow `npm install`) had to rebuild. Changing `index.js` only invalidated
the TOP layer, so nothing below it was affected.

---

## Bonus: .dockerignore
`COPY . .` busts the cache on any file change because Docker fingerprints the copied
files. A `.dockerignore` file excludes things like `node_modules` and `.git` from the
build context, so they don't needlessly bust the cache or bloat the image.

---

## What clicked for me today
- An image = a stack of read-only layers, one per instruction.
- Change a layer → that layer and everything above it rebuilds. Lower change = more rebuild.
- Order Dockerfiles by "how often does this change": stable at bottom, volatile on top.
- Dependencies are baked into the image at build time — the container never reinstalls at runtime.

---

## Quiz result
Day 3 quiz: passed after a short re-check. Corrections locked in:
- Image structure = "stack of read-only layers" (not just its contents).
- The **Engine** does the cache-checking, not the Dockerfile.
- The `npm install`-before-`COPY . .` ordering is for **cache efficiency**;
  dependencies still get baked into the final image.

**Next: Day 4 — volumes and bind mounts (data that outlives the container).**
