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
| Ship a change | Merge to `main`. CI builds multi-arch images and opens a `chore(deploy)` PR. Merge it: ArgoCD deploys. |
| Change a port, path or env var | Edit `deploy/service.yaml` |
| Run it locally without a cluster | `docker compose up` |

`deploy/service.yaml` is rendered by the platform chart in
[`vengomatto/case-interview`](https://github.com/vengomatto/case-interview).
Changes against upstream: web API base is a build-time setting (`VITE_API_BASE`, default `/api`),
Dockerfiles use `node:lts-slim`, and the broken `apps/api/k8s/` manifests are removed.
See the commit history for the exact diff.
