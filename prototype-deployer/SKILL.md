---
name: prototype-deployer
description: Use when asked to add or update CINEV/prototype-deployer based deployment in another repository. This is a bootstrap skill: it should first locate and read docs/ai from the CINEV/prototype-deployer repository, then use those docs as the source of truth before editing deploy.yaml, workflows, or Dockerfiles.
---

# Prototype Deployer Bootstrap

Read `references/bootstrap-flow.md` before making changes.

## When To Use
- A user wants to add `CINEV/prototype-deployer` based deployment to another repository.
- A user wants to repair or update an existing repository that already uses `prototype-deployer`.
- A user wants a short AI bootstrap that can discover the current deployment contract from the source repository at runtime.

## Workflow
1. Locate an accessible checkout of `CINEV/prototype-deployer`, or fetch it if the environment has permission.
2. Read `docs/ai/README.md` and then the rest of `docs/ai/*.md` from that repository.
3. Treat those docs as the source of truth for workflow rules, reusable inputs, image naming, secrets contract, Dockerfile requirements, and deployment behavior.
4. Inspect the target repository and infer service name, runtime, port, Dockerfile location, and whether ingress is needed.
5. If there is no buildable Dockerfile, create one as a multi-stage build appropriate for the runtime.
6. Create or update `deploy.yaml`.
7. Create or update `.github/workflows/deploy.yml` using the safe caller-workflow pattern described in the source docs.
8. End with a short summary of assumptions or missing values.

## Notes
- Do not hardcode private infrastructure values into this skill. Read them from `docs/ai` at runtime instead.
- If `CINEV/prototype-deployer` cannot be accessed, stop and ask the user for a local checkout path or the relevant `docs/ai` contents.
- If the user reports a build failure from this deployment system, ask for the copy-paste build failure block before guessing.
