# README

## What is DevLabs?

DevLabs is a collection of containerized labs for development convenience. Labs will include dev environments for such concerns as:

* Event-driven architectures
* Observability
* Multi-container environments
* Various useful tools and services

Key goals:

* Each lab is **self-contained** with Docker Compose and in future iterations a minimal TypeScript CLI.
* Nx manages orchestration across labs, enabling reproducible builds, scripts, and task inference.
* Monorepo structure ensures consistency while allowing independent lab development.

## DevLabs Nx

Nx is responsible for:

* **Managing project graph**: Understanding dependencies between labs or tasks.
* **Detecting and running tasks**: Picking up scripts defined in `package.json` for each lab.
* **Caching outputs**: Avoiding redundant builds or script executions.
* **Providing tooling integration**: Nx Console and CLI help run, visualize, and debug tasks.

> Nx does **not replace npm** — it leverages the existing package.json scripts, making task orchestration metadata-driven.

## What is Nx?

Nx is a modern monorepo framework that provides:

* **Task orchestration**: Executes scripts and builds across projects with an awareness of dependencies.
* **Task inference**: Automatically determines which tasks need to run based on changes in your code.
* **Caching and parallelization**: Speeds up repeated runs by skipping unchanged tasks and running independent tasks concurrently.
* **Workspace abstractions**: Organizes multiple projects/packages under a single monorepo, making dependency management, code sharing, and scripting consistent.
