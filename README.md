# Multi-Agent App Scaffold

This repo is a production-friendly starter for a multi-agentic AI service:

- FastAPI app (swap for CrewAI/LangGraph/etc.)
- Pytest + Ruff on every push/PR
- Container build
- GitHub → AWS OIDC (no long-lived AWS keys)
- CI on push/PR, CD on GitHub Release or tag `v*` to ECS Fargate

## Quickstart

```bash
# local run
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8080
```

## One-time AWS setup
1. Create ECR repo `multi-agent-app` in `ap-southeast-1`.
2. Create ECS cluster + service (`multi-agent-cluster` / `multi-agent-svc`) with Fargate + ALB to port 8080.
3. Create CloudWatch Logs group `/ecs/multi-agent-svc`.
4. Set up GitHub OIDC role `github-oidc-deployer` with permissions for ECR/ECS/PassRole.
5. Add repo secret: `AWS_ACCOUNT_ID`.

## Release → Deploy
Cut a Git tag like `v0.1.0` or publish a GitHub Release. CD workflow builds and pushes an image to ECR and updates the ECS service.
