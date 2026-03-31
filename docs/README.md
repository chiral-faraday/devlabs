# README
Nx is essentially a monorepo orchestration platform featuring:

- computation [caching](https://nx.dev/docs/features/cache-task-results) system for performant builds/runs
- project and task [graphs](https://nx.dev/docs/features/explore-graph) for task inference
- task [pipelines](https://nx.dev/docs/concepts/task-pipeline-configuration) for ordered/parallelized execution
- module [boundary enforcement](https://nx.dev/docs/features/enforce-module-boundaries)

## Principles
Nx is metadata and graph driven. Everything in Nx has metadata to enable tooling, and most metadata can be [inferred directly from existing configuration files](https://nx.dev/docs/concepts/inferred-tasks). Nx uses uses the [project graph](https://nx.dev/docs/concepts/mental-model#the-project-graph) to derive a task graph and executes the tasks in that graph. The project and task graph are not isomorphic. A given task graph can contain many different targets. Nx tasks can run in parallel; can be sequenced; and Nx ensures that when a task runs the tasks it depends on will run accordingly. 

![Parallel Process / Task Graph image](https://nx.dev/docs/_astro/task-graph-execution.BNawkWiA.svg)

## Hashing and Caching
For efficient and performant task execution, Nx computes a hash based on source files, configuration, dependencies and other inputs **before** running the task. If the hash matches a previous run, the cached result is replayed. This includes terminal outputs and other things such as file artifacts and so on. If it does not match, Nx runs the task and persists the result for the next run. Nx supports local and remote caching, checking the local cache then remote (if remote caching is configured).

## Distributed task execution
Nx supports distributed task execution allowing larger workspaces execute tasks from the task graph across multiple machines with or without caching. Remote caching makes it possible to share artifacts between [agents](https://nx.dev/docs/features/ci-features/distribute-task-execution) where needed.



## References

[How caching works](https://nx.dev/docs/concepts/how-caching-works)