# Continuous integration and deployment

The [CI/CD workflow](../.github/workflows/ci-cd.yml) runs on every push and on pull requests targeting `main`.

## Checks

| Job | Work performed |
|---|---|
| `api` | Restore, build, and run the .NET unit tests |
| `web` | Install locked dependencies, run Vitest, and create the Angular production build |
| `integration` | Run API, authentication, financial, jobs, reporting, and SMTP tests against PostgreSQL 16 |
| `e2e` | Run the Playwright workflows in Chromium against a disposable database |
| `backup-restore` | Create and restore a database and product-image backup |
| `container` | Build the production image and verify the health endpoint, SPA route, and runtime configuration |

Test results are uploaded when useful, and Playwright traces are retained on failure.

## Deployment gate

The `deploy` job starts only after all six checks pass. It runs for pushes to `DEPLOY_BRANCH`, which defaults to `main` in this repository.

To connect Render:

1. Set the Render service branch to `main`.
2. Disable Render's Git auto-deploy.
3. Create a Render deploy hook.
4. Save the hook as the GitHub Actions secret `RENDER_DEPLOY_HOOK`.
5. Push to `main` and verify the deployment in Render Events.

The hook URL is a credential and must not be committed. If the secret is absent, the deployment step is skipped while all build and test jobs still run.

A separate Vercel frontend is optional. Add a Vercel deploy hook as `VERCEL_DEPLOY_HOOK` only when maintaining a separate frontend deployment.

## Production checks

After deployment, verify:

- [Application](https://possystem-1-26mq.onrender.com/login)
- [Health endpoint](https://possystem-1-26mq.onrender.com/health)
- [Swagger UI](https://possystem-1-26mq.onrender.com/swagger/index.html)
- [OpenAPI document](https://possystem-1-26mq.onrender.com/swagger/v1/swagger.json)

The combined Docker image serves Angular and the API from the same origin. This keeps routing and refresh-cookie behavior consistent across browsers.
