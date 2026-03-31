# DevLabs Nx

## What is DevLabs?

DevLabs is a collection of containerized development labs for development convenience. Labs include dev environments for exploring :

* Event-driven architectures
* Multi-container environments
* TypeScript-based CLIs for lab orchestration
* Various useful tools and services

Key goals:

* Each lab is **self-contained** with Docker Compose and in future iterations a minimal TypeScript CLI.
* Nx manages orchestration across labs, enabling reproducible builds, scripts, and task inference.
* Monorepo structure ensures consistency while allowing independent lab development.

---

## DevLabs Nx

Nx is responsible for:

* **Managing project graph**: Understanding dependencies between labs or tasks.
* **Detecting and running tasks**: Picking up scripts defined in `package.json` for each lab.
* **Caching outputs**: Avoiding redundant builds or script executions.
* **Providing tooling integration**: Nx Console and CLI help run, visualize, and debug tasks.

> Nx does **not replace npm** — it leverages the existing package.json scripts, making task orchestration metadata-driven.

---

## 1. What is Nx?

Nx is a modern monorepo framework that provides:

* **Task orchestration**: Executes scripts and builds across projects with an awareness of dependencies.
* **Task inference**: Automatically determines which tasks need to run based on changes in your code.
* **Caching and parallelization**: Speeds up repeated runs by skipping unchanged tasks and running independent tasks concurrently.
* **Workspace abstractions**: Organizes multiple projects/packages under a single monorepo, making dependency management, code sharing, and scripting consistent.

---

## Guide to Creating a DevLab with Nx
The content below summarise how the `devlabs` monorepo was created.

### Create Nx Workspace

```bash
npx create-nx-workspace@latest devlabs --preset=npm --interactive=false
cd devlabs
```

* Creates the monorepo skeleton repository configured with NPM workspaces
* Top-level `package.json` is initialized with sensible defaults and the name was manually  :

```json
{
  "name": "@chiral/devlabs",
  "private": true,
  "workspaces": ["packages/*"]
}
```

* Run `npm install` to populate `node_modules`.

---

### Step 1: Create the first lab project

* Directory structure for the first lab (`diagramming`):

```
packages/diagramming/
├── compose.yaml
├── config/tsconfig.base.json
├── docker/Dockerfile
├── package.json
├── scripts/
└── tsconfig.json
```

* `scripts/` will hold the TypeScript CLI for lab management.

---

### Step 2: Define `package.json` for the lab

```json
{
  "name": "@chiral/devlabs/diagramming",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "tsc -b",
    "test": "jest",
    "docker:up": "docker compose -f compose.yaml up -d",
    "docker:down": "docker compose -f compose.yaml down",
    "docker:logs": "docker compose -f compose.yaml logs -f",
    "cli": "ts-node scripts/index.ts"
  },
  "devDependencies": {
    "typescript": "~5.9.2",
    "ts-node": "^10.9.1",
    "jest": "^29.0.0",
    "@types/jest": "^29.0.0"
  }
}
```

* All scripts are automatically picked up by Nx as **targets**.

---

### Step 3: Verify with Nx Console

* Open Nx Console in your IDE.

* Confirm the following targets in the Console UI:

  * `build`
  * `test`
  * `docker:up`
  * `docker:down`
  * `docker:logs`

* Run targets from the console or CLI to verify:

```bashl
nx run @chiral/devlabs/diagramming:build
nx run @chiral/devlabs/diagramming:docker:up
```

* Nx will execute the tasks and track dependencies for future caching.

---

### Step 4: Next Steps (Scalable Workflow)

* Repeat the lab creation process for additional labs.
* Nx automatically builds the **project graph** across labs.
* Task inference ensures only changed projects/scripts are re-executed.
* TypeScript CLIs in `scripts/` can later be refactored into Nx libraries if needed.

---

**Outcome:**

* A fully Nx-controlled monorepo.
* Docker and TypeScript CLIs integrated with workspace-level orchestration.
* Ready to scale to multiple labs with task inference, caching, and reproducible builds.

