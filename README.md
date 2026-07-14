# oeds-crawler-pack

Local staging repository for preserving KIT crawler selection behavior while
upstream OEDS remains the base project.

## Responsibility

This module provides the crawler registry layer for KIT-enhanced crawlers. It
does not own the shared crawler base, scheduler, post-run scripts, or
deployment.

`crawler_core` belongs in OEDS core. Shared crawler runtime behavior such as
`BaseCrawler`, database URI handling, metadata helpers, and the crawler contract
must be fixed there so the same crawlers can be used by plain OEDS, a slim
module setup, or the full deployment stack.

This package exists to prevent functionality loss while improved KIT crawlers
are prepared for upstream OEDS or kept as extension crawlers.

## Current Behavior

- discovers class-based KIT crawlers from the current KIT checkout
- exposes stable crawler specs such as `crawler.smard:SmardCrawler`
- gives KIT crawler specs priority over upstream OEDS when the deployment stack
  merges registries
- keeps upstream-only crawlers available through `oeds-core`

The registry priority is:

```text
oeds-crawler-pack before oeds-core
```

## Local Development

Run from the parent workspace:

```powershell
python .\modular_repos\tools\verify_modules.py
```

The verifier checks that important KIT crawlers such as `smard`, `entsoe_fms`,
and `weather_forecast` are discovered and preferred where expected.

Standalone CI runs fixture-based registry tests without requiring a checked-out
KIT workspace. The real KIT crawler discovery assertion is covered by the
parent workspace verifier.

## Policy

No crawler should be removed from the deployable crawler set until it has one of
these documented outcomes:

- merged into upstream OEDS
- kept here as an extension crawler
- replaced by a verified equivalent
- explicitly deprecated

## Out Of Scope

- shared `BaseCrawler` or `crawler_core` implementation
- scheduler UI
- deployment playbooks
- post-run gapfill or forecast pipelines
