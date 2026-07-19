# Upload UI Evidence

Upload screenshots, videos, HTML reports, traces, and logs from GitHub Actions to a private
[UI Evidence Portal](https://github.com/mtzack-org/ui-evidence-portal). Every upload adds a direct
Portal link to the GitHub Actions Job Summary.

## Usage

```yaml
- name: Upload UI evidence
  if: always()
  uses: mtzack-org/upload-ui-evidence@v1
  with:
    portal-url: ${{ secrets.UI_EVIDENCE_PORTAL_URL }}
    token: ${{ secrets.UI_EVIDENCE_INGEST_TOKEN }}
    platform: web
    status: ${{ job.status }}
    screenshots: test-results/**/*.png
    videos: test-results/**/*.webm
    reports: playwright-report/**/*.{html,zip}
    traces: test-results/**/*.zip
    logs: test-results/**/*.{log,txt,json}
    if-no-files-found: error
```

Set `UI_EVIDENCE_PORTAL_URL` to the deployed Portal origin. Set
`UI_EVIDENCE_INGEST_TOKEN` to the same secret as the Portal's `EVIDENCE_INGEST_TOKEN`.
Store both as GitHub repository or organization secrets.

Artifact inputs accept newline-separated glob patterns:

```yaml
screenshots: |
  test-results/**/*.png
  screenshots/**/*.jpg
```

The Action supports `web`, `android`, and `ios`. It exposes `run-id`, `run-url`, and
`uploaded-count` outputs. Optional result inputs are `total`, `passed`, `failed`, `skipped`, and
`duration-ms`.

When no files match, `if-no-files-found` controls whether the Action emits a warning (`warn`, the
default), fails (`error`), or remains silent (`ignore`). No empty Portal run is created.

For a Vercel Preview protected by Deployment Protection, pass an Automation Bypass secret through
`vercel-protection-bypass`. Do not set it for an unprotected production Portal.

## Deploy your Portal

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fmtzack-org%2Fui-evidence-portal)

Connect a Vercel Private Blob store and configure the Portal environment variables after deployment.
The Portal repository contains the complete setup instructions.

## Development

Node.js 24 is required.

```bash
npm ci
npm test
npm run build
```

Commit `dist/` after changing the Action source. GitHub runners execute the committed bundle.

## Security and support

Read [`SECURITY.md`](SECURITY.md) before reporting a vulnerability. For reproducible bugs and feature
requests, follow [`SUPPORT.md`](SUPPORT.md).

## License

GNU Affero General Public License v3.0 only (`AGPL-3.0-only`). See [`LICENSE`](LICENSE).
