---
name: gitlab-ci-author
description: Write, debug, and optimize GitLab CI/CD pipeline configuration files.
---

# GitLab CI/CD author

## When to use

- Creating or editing `.gitlab-ci.yml` files
- Debugging failing pipelines
- Optimizing CI/CD performance

## Instructions

1. Use `rules:` instead of `only:`/`except:` for job control.
2. Use `needs:` to define a directed acyclic graph (DAG) for faster pipelines.
3. Cache dependencies with `cache:` and use `key: files:` for lock-file-based cache keys.
4. Store build outputs with `artifacts:` and set appropriate `expire_in:` values.
5. Use CI/CD variables for secrets — never hardcode tokens or credentials.
6. Prefer `include:` and CI/CD components for reusable configuration.
