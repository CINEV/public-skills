# Prototype Deployer Bootstrap Flow

This public skill is intentionally thin. It does not carry the full deployment contract inside the skill itself.

## Why
- `CINEV/prototype-deployer` may contain organization-specific deployment rules.
- Those rules change over time.
- The current contract should come from the source repository's `docs/ai`, not from stale copied text in a public skill.

## Required First Step
Before making changes in a target repository, the agent should obtain and read:

1. `CINEV/prototype-deployer/docs/ai/README.md`
2. `CINEV/prototype-deployer/docs/ai/00_overview_and_architecture.md`
3. `CINEV/prototype-deployer/docs/ai/01_config_parsing_and_validation.md`
4. `CINEV/prototype-deployer/docs/ai/02_manifest_generation.md`
5. `CINEV/prototype-deployer/docs/ai/03_gitops_and_pipelines.md`

## Access Strategy
Use the first option that works:

1. A local checkout path already provided by the user
2. A sibling checkout that already exists on disk
3. Authenticated `git` or `gh` access to `https://github.com/CINEV/prototype-deployer`

If none of these work, stop and ask the user for:
- a local checkout path, or
- a copy of the `docs/ai` contents

Do not invent workflow rules or internal settings when the source docs are unavailable.

## After Reading `docs/ai`
Once the source docs are available, the agent should:

1. Inspect the target repository
2. Infer service name, port, runtime, Dockerfile path, and ingress needs
3. Create a multi-stage Dockerfile if needed
4. Create or update `deploy.yaml`
5. Create or update a safe `.github/workflows/deploy.yml`
6. Keep edits focused on deployment config, workflow files, and Dockerfile unless the user explicitly asks for app code changes

## Public Skill Boundary
This public skill may mention the source repository name `CINEV/prototype-deployer`, but it should not embed private hostnames, internal IPs, registry addresses, or other sensitive infrastructure values directly.

## Build Failure Handoff
If container build fails in the target repository, ask the user for the copy-paste build failure block emitted by `prototype-deployer`.
