# CI/CD

The common CI/CD vocabulary -- the pipelines that define a build, the runs that execute them, the jobs inside a run, the artifacts they produce, the environments they deploy to and the deployments themselves -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged CI/CD, Continuous Integration, Continuous Delivery, Continuous Deployment, Deployment, Build Automation, DevOps in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| pipeline | 38 | 34 | name 88%, id 76%, created_at 59% | pipelines 20, workflows 16, pipeline 5, definitions 2 |
| run | 27 | 24 | status 83%, id 79%, created_at 67%, started_at 58% | runs 16, builds 14, executions 6, run 5 |
| job | 9 | 7 | name 71%, created_at 57%, id 57%, status 57% | jobs 6, stages 3, steps 2 |
| artifact | 14 | 12 | id 83%, name 58%, size 50% | artifacts 13, artifact 2 |
| environment | 10 | 7 | name 100%, id 86%, created_at 71%, description 57% | environments 9, envs 1 |
| deployment | 54 | 48 | id 65%, status 60% | deployments 39, deploy 15, deploys 6, deployment 4 |

Cohort: 241 providers with harvested OpenAPI, 81 crosswalks. Of the providers exposing a pipeline: 58% create, 90% list, 79% fetch, 40% update, 47% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- The hierarchy is shared: a pipeline (name, id) runs as a run (status, id, start time), which is made of jobs (name, status). GitHub Actions' workflows and runs, GitLab's pipelines and jobs, Buildkite's builds and Azure's definitions all fit it.
- A deployment agrees only on id and status: hosting platforms, CD tools and release trackers each record a deploy differently, and its environment is referenced by under a third.
- An environment agrees on name, id, description and created_at wherever it is a resource.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- A pipeline covers workflows, build definitions and build types; a run covers builds, workflow runs and executions. The two levels are kept apart because every provider keeps them apart.
- Singular `/workflow` is not a pipeline (business automation); plural `workflows` is.
- `POST /deploy` on a hosting platform creates a deployment and counts as one.
- Test-session builds (BrowserStack) are not CI runs.
- Duplicates merged: Azure DevOps, Platform.sh into Upsun.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
