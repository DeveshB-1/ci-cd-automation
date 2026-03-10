# CI/CD Automation Templates

Production-grade CI/CD pipeline templates and automation configurations.

## Contents

| File | Description |
|---|---|
| `.gitlab-ci.yml` | Full GitLab CI/CD pipeline — build, test, deploy with quality gates |
| `Jenkinsfile` | Jenkins declarative pipeline with staging/production gates |
| `Dockerfile` | Optimized Python application container |
| `k8s/deployment.yaml` | Kubernetes deployment with health checks and resource limits |

## Pipeline Flow

```
Code Push → Build Docker Image → Run PyTest → Deploy Staging → [Manual Gate] → Deploy Production
```

## Stack

`GitLab CI/CD` `Jenkins` `Docker` `Kubernetes` `Python` `PyTest`
