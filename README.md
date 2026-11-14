# 🔒 EdgeIQ Security Scanner Action

Automated security scanning for GitHub repositories. Posts grade badges and findings directly on pull requests.

[![GitHub Marketplace](https://img.shields.io/badge/GitHub-Marketplace-blue?logo=github)](https://github.com/marketplace/actions/edgeiq-security-scan)

## Quick Start

```yaml
# .github/workflows/security-scan.yml
name: Security Scan
on: [pull_request, push]

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: EdgeIQ-Labs/edgeiq-security-action@v1
        with:
          api-key: ${{ secrets.EDGEIQ_API_KEY }}
```

## Get Your Free API Key

Visit [edgeiqlabs.com/developers](https://edgeiqlabs.com/developers/) to get a free API key instantly. No credit card required.

| Tier | Rate Limit | Price |
|------|-----------|-------|
| Free | 10 req/min | $0 |
| Pro | 100 req/min | $19/mo |
| Enterprise | 1000 req/min | $99/mo |

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `api-key` | ✅ | — | EdgeIQ API key |
| `github-token` | ❌ | `secrets.GITHUB_TOKEN` | Token for posting PR comments |
| `fail-on-grade` | ❌ | _(never fail)_ | Fail check if grade below threshold (A/B/C/D/F) |
| `scan-url` | ❌ | Current repo | Override repository URL to scan |

## Outputs

| Output | Description |
|--------|-------------|
| `grade` | Security grade (A-F) |
| `score` | Security score (0-100) |
| `findings-count` | Number of findings |
| `report-url` | Link to full interactive report |

## Examples

### Basic — Scan on Every PR

```yaml
- uses: EdgeIQ-Labs/edgeiq-security-action@v1
  with:
    api-key: ${{ secrets.EDGEIQ_API_KEY }}
```

### Strict — Block Merges Below Grade B

```yaml
- uses: EdgeIQ-Labs/edgeiq-security-action@v1
  with:
    api-key: ${{ secrets.EDGEIQ_API_KEY }}
    fail-on-grade: B
```

### Scheduled — Daily Security Audit

```yaml
name: Daily Security Audit
on:
  schedule:
    - cron: '0 9 * * 1-5'  # Weekdays at 9am UTC

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: EdgeIQ-Labs/edgeiq-security-action@v1
        with:
          api-key: ${{ secrets.EDGEIQ_API_KEY }}
```

### Multi-Repo — Scan External Dependencies

```yaml
- uses: EdgeIQ-Labs/edgeiq-security-action@v1
  with:
    api-key: ${{ secrets.EDGEIQ_API_KEY }}
    scan-url: https://github.com/some-org/critical-dependency
```

## What It Checks

- 🔑 Leaked secrets and API keys (8 regex patterns)
- 📄 Missing or incomplete `.gitignore`
- 🔐 Supabase RLS policies
- ⚙️ CI/CD security (unpinned actions, exposed secrets)
- 📦 Dependency vulnerabilities
- 🕐 Stale repository detection
- 🏗️ Repository structure best practices

## Sample PR Comment

The action posts a comment like this on every PR:

> ## 🟡 EdgeIQ Security Scan: **Grade B** (78/100)
>
> | Severity | Finding |
> |----------|---------|
> | HIGH | Unpinned GitHub Action `actions/checkout@main` |
> | MEDIUM | `.env.example` committed with real values |
> | LOW | No CODEOWNERS file |
>
> 🔗 [View full report →](https://edgeiqlabs.com/free-repo-scanner/)

## Pricing

Free tier includes 10 scans/minute — enough for most projects. Upgrade to Pro ($19/mo) for higher rate limits and priority support.

→ [Get API Key](https://edgeiqlabs.com/developers/)
→ [Compliance Reports](https://edgeiqlabs.com/compliance-report/)
→ [White-Label Reports](https://edgeiqlabs.com/whitelabel-reports/)

## Support

- 📧 support@edgeiqlabs.com
- 💬 [Discord Community](https://discord.gg/PaP7nsFUJT)
- 📖 [API Documentation](https://edgeiqlabs.com/developers/)

---

Built by [EdgeIQ Labs](https://edgeiqlabs.com) — Cybersecurity tools for developers and MSPs.
