Absolutely — here’s a concise **“Golden Path Checklist”** for creating new labs in your Nx monorepo. This captures everything we’ve validated so far and ensures repeatability.

---

# DevLabs Nx Monorepo – Golden Path Checklist for New Labs

**Purpose:** Provide a repeatable process to add a new lab to the Nx-managed monorepo, ensuring task inference, Nx Console recognition, and a clean workspace structure.

---

## Create the Lab Directory

* Location: `packages/<lab-name>/`
* Minimum structure:

```
<lab-name>/
├── compose.yaml           # Docker Compose configuration
├── config/tsconfig.base.json  # Optional shared TS config
├── docker/Dockerfile      # Container build instructions
├── package.json           # Lab-specific scripts + dependencies
├── scripts/               # TypeScript CLI implementation
└── tsconfig.json          # Project-specific TS config
```

---

## Define `package.json` for the Lab

* Set `name` as `@chiral/devlabs/<lab-name>`
* Ensure `private: true`
* Include scripts for Nx task inference:

```json
{
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

> Nx will automatically detect these scripts as targets for task inference.

---

## Configure TypeScript

* `tsconfig.json` should include `scripts/` folder:

```json
{
  "extends": "./config/tsconfig.base.json",
  "compilerOptions": {
    "outDir": "dist",
    "rootDir": "scripts"
  },
  "include": ["scripts/**/*"]
}
```

---

## Verify Nx Integration

* Open **Nx Console** in your IDE.

* Confirm the lab appears as a project, and scripts appear as targets:

  ```
  build, test, docker:up, docker:down, docker:logs, cli
  ```

* Test short-name and scoped commands:

```bash
nx run <lab-name>:build
nx run @chiral/devlabs/<lab-name>:docker:up
```

---

## Next Steps for the Lab

* Add lab-specific dependencies (`npm install <package>`).
* Implement the TypeScript CLI in `scripts/`.
* Expand Docker Compose and Dockerfile as needed.
* Nx caching and task inference will now automatically track changes in this lab.

---

## Key Principles

* **Monorepo scope (`@chiral/devlabs`)** remains consistent.
* **Nx controls orchestration**, not npm scripts — scripts are merely targets for task inference.
* **Repeatable structure** ensures consistency across labs.
* **Short-name commands** are convenient; scoped names avoid ambiguity in multi-lab setups.
* The **golden path** emphasizes validation first — Nx recognition, targets, and Console visibility — before adding dependencies or implementing complex CLI logic.
