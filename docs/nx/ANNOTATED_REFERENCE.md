# ANNOTATED_REFERENCE
Description: annotated reference for Nx

## When to use package.json vs project.json
Both `package.json` and `project.json` are used to configure Nx targets and both support the same configuration options and executors. The choice between `package.json` and `project.json` is a matter of preference. Concretely, the difference is in how the relevant file signals to Nx its role:

- `package.json`: use the `nx` property to define targets with executors, options, and other Nx-specific config
- `project.json`: a dedicated configuration file that allows `package.json` to focus on package metadata

[When to use package.json vs project.json](https://nx.dev/docs/reference/project-configuration#when-to-use-packagejson-vs-projectjson)

## Working with Tasks
A task is, simply, a named action that Nx can run for a given project or multiple projects. Tasks defined in a project's scripts (`package.json`) are picked up automatically by Nx. Tasks can be defined in a `project.json` file for a given project to decouple the package manifest and project orchestration. 

## Working with Task Dependencies
In the context of a monorepo, tasks are typically expected to run in a well-defined order. This can be at the project level and at the cross-project level. Task dependencies are represented in the following example `nx.json` with the caret (`^`):

```json
{
  "targetDefaults": {
    "build": {
      "dependsOn": ["^build"],
    },
  },
}
```
The caret signals to Nx that the same task should be executed on projects the relevant project depends on. Task dependencies within a project are signalled by omitting the caret either in the `package.json` or the `project.json`. For example, in a sample project where this configuration is defined on the `package.json`:

```json
{
  "scripts": {
    "build": "vite build",
    "generate-api-types": "openapi-generator generate -i api.yaml -o src/api",
  },
  "nx": {
    "targets": {
      "build": {
        "dependsOn": ["generate-api-types"],
      },
    },
  },
}
```