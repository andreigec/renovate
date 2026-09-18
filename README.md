# renovate

Shared Renovate preset for all repos. Each repo extends it with:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "labels": ["<repo>"],
  "extends": ["github>andreigec/renovate"]
}
```

## Schedule

Updates run once a week per repo, staggered by day (Australia/Melbourne):

| Day       | Repos                                                        |
| --------- | ------------------------------------------------------------ |
| Monday    | `ag-*`, `renovate`, `eslint-config-e7npm`                    |
| Wednesday | `analytica.click`, `analytica.click.gh`, `surveyfoundry.com` |
| Friday    | everything else                                              |

Lock file maintenance runs on Sunday.

The grouping is defined centrally in `default.json` via `matchRepositories`, so a
new repo only needs the two-line config above and automatically lands in the
Friday group.

## Automerge

PRs are grouped into `all` (third party) and `in house`, and are merged
automatically once their CI checks are green.

Automerge is performed by Renovate itself (`platformAutomerge: false`) because
the private repos cannot use GitHub branch protection / required status checks
(paid feature) and native auto-merge is disabled on them. Renovate polls the
branch and only merges when its status is `green`.

Repos with no `pull_request` workflow (`brainfony`, `rpi-mega`) have no checks, so
Renovate treats them as green and merges immediately. Add a PR check or disable
automerge for those repos if you want a gate.

Run the validator locally with `pnpm -w run lint`.
