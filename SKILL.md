# skillhub-registry

---

## name: skillhub-registry

description: Use this when you need to search, inspect, install, or publish agent skills against a SkillHub registry. SkillHub is a skill registry with a ClawHub-compatible API layer, so prefer the `clawhub` CLI for registry operations instead of making raw HTTP calls.

# SkillHub Registry

Use this skill when you need to work with a SkillHub registry: search skills, inspect metadata, install a package, or publish a new version.

> Important: Prefer the `clawhub` CLI for registry workflows. SkillHub exposes a ClawHub-compatible API surface and a discovery endpoint at `/.well-known/clawhub.json`, so the CLI is the safest path for auth, resolution, and download behavior. Only fall back to raw HTTP when debugging the server itself.

## What SkillHub Is

SkillHub is an enterprise-oriented skill registry. It stores versioned skill packages, supports namespace-based skill management, and keeps `SKILL.md` compatibility with OpenSkills-style packages.

Key facts:

- Internal coordinates use `@{namespace}/{skill_slug}`.
- If using the clawhub CLI, the compatible format is `{namespace}--{skill_slug}`.
- `latest` always means the latest published version, never draft or pending review.
- Public skills in `@global` can be downloaded anonymously.
- If no namespace is specified, it defaults to `@global`.
- `{skill_slug}` can be used instead of `global--{skill_slug}`
- Team namespace skills and non-public skills require authentication.

## Configure The CLI

Point `clawhub` at the SkillHub base URL:

```bash
export CLAWHUB_REGISTRY=https://skillhub.eccom.com.cn
```

Alternatively:

```bash
npx clawhub install my-skill --registry https://skillhub.eccom.com.cn
```

If you need authenticated access:

```bash
clawhub login --token sk_your_api_token_here
```

Verify:

```bash
curl https://skillhub.eccom.com.cn/.well-known/clawhub.json
# Expected: {"apiBase":"/api/v1"}
```

## Coordinate Rules

| SkillHub coordinate   | clawhub slug          |
| :-------------------- | :-------------------- |
| `@global/my-skill`    | `my-skill`            |
| `@team-name/my-skill` | `team-name--my-skill` |

## Common Workflows

### Search

```bash
npx clawhub search email
npx clawhub search ""
```

### Inspect

```bash
npx clawhub info my-skill
```

### Install

```bash
npx clawhub install my-skill
npx clawhub install my-skill@1.2.0
npx clawhub install team-name--my-skill
```

### Publish

```bash
npx clawhub publish ./my-skill
```

## API Fallback

When clawhub CLI is unavailable, use the REST API directly:

```bash
# Discover
curl https://skillhub.eccom.com.cn/.well-known/clawhub.json

# List all skills
curl https://skillhub.eccom.com.cn/api/v1/skills

# Skill detail — replace with actual endpoint
```

## Authentication

- `@global` + `PUBLIC`: anonymous access allowed
- Team namespace: requires authentication
- Write operations always require authentication
