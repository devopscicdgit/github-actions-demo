<div align="center">

# GitHub Actions Demo (Next.js + Pages)

Automated build, test, and deployment pipeline using GitHub Actions. Adaptable blueprint for data engineering teams establishing CI/CD, quality gates, and governance around application + workflow code.

[![Next.js](https://img.shields.io/badge/next.js-13.5-black.svg)](https://nextjs.org/) [![React 18](https://img.shields.io/badge/react-18.2-61dafb.svg?logo=react&logoColor=61dafb&label=react)](https://react.dev/) [![Node.js](https://img.shields.io/badge/node.js-20.x-43853d.svg?logo=node.js&logoColor=white)](https://nodejs.org/) [![GitHub Actions](https://img.shields.io/badge/CI-GitHub_Actions-blue.svg?logo=githubactions&logoColor=white)](https://docs.github.com/en/actions) [![GitHub Pages](https://img.shields.io/badge/deploy-GitHub_Pages-222222.svg?logo=github&logoColor=white)](https://pages.github.com/) [![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE.txt)

</div>

## 1. Overview
This repo demonstrates a minimal **Next.js** application deployed to **GitHub Pages** via multi‑stage GitHub Actions workflows (`build`, `test`, `deploy`). While currently a simple web app, the structure and automation patterns map cleanly to data engineering projects (ETL/ELT pipelines, data services, scheduled jobs) that need:
- Repeatable environment setup
- Deterministic builds & artifact promotion
- Testing & linting gates
- Isolated environments (dev/test/prod)
- Observability, security, and compliance hooks

## 2. Tech Stack
| Layer | Choice | Notes |
|-------|--------|-------|
| Framework | Next.js 13.5 | App/site scaffold (could analogize to pipeline orchestrator UI) |
| Language | JavaScript / React | Easily replace with TypeScript for type safety |
| CI/CD | GitHub Actions | Workflows in `demo-files/` (normally `.github/workflows/`) |
| Deployment Target | GitHub Pages | Static export in `./out` |

## 3. Repository Structure
```
pages/              # Next.js pages (UI + API route example)
styles/             # CSS modules & global styles
public/             # Static assets
demo-files/         # Example GitHub Actions workflows (relocate to .github/workflows/ in production)
hello_world.txt     # Simple artifact used in hello-world workflow step
package.json        # Scripts: dev/build/start/test (lint)
next.config.js      # Next.js configuration for static export
```

### Suggested Additions for Data Engineering Projects
```
src/pipelines/      # ETL/ELT job definitions (dbt, Airflow DAGs, custom code)
src/lib/            # Shared utilities (I/O, validation, logging)
tests/              # Unit/integration tests (data contract tests)
schemas/            # Data models (JSON Schema, Avro, Protobuf)
docs/               # Architecture & runbooks
.github/workflows/  # Production workflow definitions
```

## 4. GitHub Actions Workflows
Two example YAML files are provided in `demo-files/`:
1. `hello-world.yml` – basic workflow echoing file content.
2. `build-test-deploy.yml` – multi-job pipeline:
	- build: install + compile
	- test: lint gate (extend with unit tests)
	- deploy: configure Pages, build, upload artifact, deploy.

### Best Practices for Data Engineering CI/CD
- Trigger granularity: use `on: push` + filters (`paths`, `branches`) to avoid unnecessary runs for doc changes.
- Separation of concerns: distinct jobs for build, quality validation, data contract tests, packaging, deploy.
- Caching: add `actions/setup-node` cache or custom caches for dependency & dataset snapshots.
- Artifact lineage: publish build manifest (hashes, versions) to enable reproducibility.
- Promotion: deploy from tagged releases or protected branches after validations pass.
- Secrets management: use GitHub Actions secrets & OIDC for cloud access (avoid static credentials).

## 5. Local Development
Prerequisites: Node.js 20.x (or version pinned in workflow). Recommended: `nvm` or Volta for version enforcement.

Install & run:
```
npm install
npm run dev
```
Visit: http://localhost:3000

### Adding TypeScript (recommended)
```
npm install --save-dev typescript @types/react @types/node
npx tsc --init
```
Update files to `.ts/.tsx` incrementally. Add a `type-check` script + CI job.

## 6. Development Workflow
1. Create feature branch: `feat/<short-description>`.
2. Implement changes + tests.
3. Run lint & unit tests locally: `npm test`.
4. Commit with conventional message: `feat: add data ingestion step`.
5. Open PR → automatic CI runs.
6. PR review (architecture, tests, security).
7. Merge via squash or rebase for linear history.

## 7. Testing Strategy
Current: lint only (via `npm test`).

Recommended Enhancements:
- Unit tests: Jest/Vitest for transformation logic.
- Contract tests: validate schemas vs. sample payloads.
- Integration tests: run pipeline against ephemeral test dataset.
- Data quality checks: row counts, null ratios, distribution drift.
- Performance regression: capture execution time & resource usage.

## 8. Deployment & Release
For static Next.js export: ensure `next.config.js` sets `output: export` (add if missing) and artifacts land in `out/`.

For data pipelines/services:
- Tag releases: `vX.Y.Z` triggers production deploy.
- Parameterize deploy job with environment matrix (dev, staging, prod).
- Use OIDC to assume cloud roles for infrastructure provisioning.

## 9. Security & Compliance
- Dependency scanning: enable Dependabot & `npm audit` job.
- Secret scanning: ensure GitHub secret scanning turned on.
- SBOM: generate CycloneDX (`npm exec @cyclonedx/cyclonedx-npm`) artifact.
- Least privilege: scoped tokens for Pages deploy only.
- Data governance: add schema validation step before merge for pipeline repos.

## 10. Observability
- Logging: standard JSON structured logs for pipeline steps.
- Metrics: emit counts & latency to monitoring system (e.g., Prometheus) via separate action step.
- Tracing: wrap critical transformations with OpenTelemetry spans.
- Alerting: define thresholds (schema change failure, row count anomaly). Integrate with Slack/Teams via action.

## 11. Performance & Cost Controls
- Cache dependencies to reduce build time.
- Avoid large dataset downloads on every PR—use sampled fixtures.
- Scheduled workflows limited with `if:` conditions to prevent overlap.

## 12. Contributing
See `CONTRIBUTING.md` (to be added). Key points:
- Discuss major changes via issue.
- Keep PRs small & focused.
- Include tests & docs for new features.

## 13. License
See `LICENSE.txt` for license details (original course template).

## 14. Roadmap / Next Steps
- Add TypeScript + strict linting.
- Add Jest tests + coverage gate.
- Migrate workflows to `.github/workflows/`.
- Implement caching & SBOM generation.
- Add schema directory + validation job example.
- Introduce data quality test harness.

## 15. Quick Start (TL;DR)
```
git clone <your-fork-url>
cd github-actions-demo
npm install
npm run dev
# Open PRs → CI builds, tests, deploys on merge/tag
```

---
Original content: Pluralsight course template reference retained for attribution.

