# Testkube Sample Application

A sample 3-tier application to run tests on.
The application is composed of a React frontend, NodeJs backend and a PostgreSQL database.

It can be used to showcase Testkube tests workflows.

![Testkube Sample Application](./docs/images/app.png)

## Running the application

You can either use docker-compose.yaml:

```
docker-compose up
```

Or run it locally:

```
npm run start
```

When running locally you should start your own PostgreSQL database.
There are many ways to do this, one suggestion is to use Docker:

```
docker run --name testkube-sample-db \
  -p 15432:5432 \
  -e POSTGRES_DB=api-db \
  -e POSTGRES_USER=api-user \
  -e POSTGRES_PASSWORD=api-password \
  --rm -d postgres
```

## Running tests

Unit tests can be executed without a running appliction:

```
npm run test
```

For E2E test, you should first run the application as described above:

```
npm run test:e2e
```

---

## Deploying to Kubernetes

This repo carries its own deploy flow. You do not need to know Kubernetes.

| You want to | You do |
|---|---|
| Ship a change | Open a PR (CI runs the tests and builds both images without pushing), merge it. After the merge, CI builds multi-arch images, checks that both architectures exist, and merges a `chore(deploy)` PR for you. Argo CD deploys. To roll back, revert that PR. |
| Change a port, path or env var | Edit `deploy/service.yaml` |
| Run it locally without a cluster | `docker compose up` |

Work on a branch and open a PR even when nobody reviews it. The PR run proves the tests pass and both
images build before anything reaches `main`, and it keeps one reviewable unit per change.

To roll back, click **Revert** on the deploy PR and merge the revert PR (the PR comment lists the previous tag).
The full procedure is the **Rollback** section of the platform repo's `docs/runbook.md`.

`deploy/service.yaml` is rendered by the platform chart in
[`vengomatto/case-interview`](https://github.com/vengomatto/case-interview).
Changes against upstream: web API base is a build-time setting (`VITE_API_BASE`, default `/api`),
Dockerfiles use `node:lts-slim`, and the broken `apps/api/k8s/` manifests are removed.
See the commit history for the exact diff.
