# renovate-config

The **single source of truth** for cross-cutting
[Renovate](https://docs.renovatebot.com/) decisions and
conventions across all serverless-ca repositories.

This repository works with the
[Renovate GitHub App](https://github.com/apps/renovate)
(hosted by Mend). Each consumer repo extends the shared
presets via `"extends"` in its `renovate.json`.

## Quick start

Extend the shared preset in your repo's `renovate.json`:

```json
{
  "extends": ["github>serverless-ca/renovate-config"]
}
```

Then add the presets that match your dependency types.

## Shared presets

| Preset | Extends as | Description |
|---|---|---|
| `default.json` | `github>serverless-ca/renovate-config` | Base settings: `config:recommended`, dashboard, labels, `platformAutomerge`, OSV vulnerability alerts |
| `github-actions.json` | `…:github-actions` | GitHub Actions: SHA-pins external actions, skips `serverless-ca/*` internal workflows, 7-day cooldown, weekly Monday schedule, automerge all (minor/patch/major) |
| `python-packages.json` | `…:python-packages` | Python `pip_requirements`: 7-day cooldown, weekly, automerge minor/patch, major requires review. Dev packages use `chore` commit type. |
| `terraform-providers.json` | `…:terraform-providers` | Terraform providers: 7-day cooldown, weekly, automerge minor/patch, major requires review |
| `internal-terraform-modules.json` | `…:internal-terraform-modules` | `serverless-ca/ca/aws` Terraform module: auto-update with no cooldown |

## Consumer examples

### terraform-aws-ca

```json
{
  "extends": [
    "github>serverless-ca/renovate-config",
    "github>serverless-ca/renovate-config:github-actions",
    "github>serverless-ca/renovate-config:python-packages"
  ]
}
```

### cloud-ca

```json
{
  "extends": [
    "github>serverless-ca/renovate-config",
    "github>serverless-ca/renovate-config:github-actions",
    "github>serverless-ca/renovate-config:python-packages",
    "github>serverless-ca/renovate-config:internal-terraform-modules"
  ]
}
```

### alb / api-gateway

```json
{
  "extends": [
    "github>serverless-ca/renovate-config",
    "github>serverless-ca/renovate-config:terraform-providers"
  ]
}
```

## PR Validation

`.github/workflows/ci.yml` runs on every PR:

- **Validate** — runs `renovate-config-validator --strict
  --no-global` on each preset JSON file

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for implementation
and verification workflows.
