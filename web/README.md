# CounterPOS frontend

Angular 22 application for the register, shifts, sales, dashboard, inventory, purchasing, customers, history, and settings screens.

## Development

```bash
npm ci
npm start
```

The development server runs at `http://localhost:4200`. Requests to `/api` and `/health` are forwarded to the ASP.NET Core API at `http://127.0.0.1:5080` through `proxy.conf.json`.

## Tests

```bash
npm test -- --watch=false
npx playwright install chromium
npm run e2e
```

Playwright starts its own API and expects the disposable PostgreSQL connection configured by `POS_E2E_CONNECTION`.

## Production build

```bash
POS_API_ORIGIN=https://api.example.com npm run build
```

Use `POS_API_ORIGIN=same-origin` when the SPA is hosted by the API container.
