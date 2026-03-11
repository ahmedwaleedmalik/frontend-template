# Frontend Template

Opinionated React template with tooling pre-configured. Clone and start building.

## Tech Stack

| Layer        | Choice                         | Version |
| ------------ | ------------------------------ | ------- |
| Framework    | React                          | 19      |
| Language     | TypeScript (strict)            | 5.9     |
| Build        | Vite                           | 7       |
| Styling      | Tailwind CSS                   | 4       |
| Components   | shadcn/ui                      | —       |
| Server State | TanStack Query                 | 5       |
| Client State | Zustand                        | 5       |
| Routing      | TanStack Router                | 1       |
| Testing      | Vitest + React Testing Library | —       |
| Linting      | ESLint (flat, type-checked)    | 9       |
| Formatting   | Prettier                       | 3       |
| Git Hooks    | husky + lint-staged            | —       |
| CI           | GitHub Actions                 | —       |
| Container    | nginx-unprivileged (alpine)    | —       |
| Deploy       | Helm chart                     | —       |

## Project Structure

```
src/
├── api/                  # API client, query keys
├── components/
│   ├── layout/           # Shell, sidebar, header
│   ├── ui/               # shadcn/ui components (add via npx shadcn)
│   └── common/           # Error boundary, not-found, shared components
├── hooks/                # Custom hooks
├── lib/                  # Utilities (cn, etc.)
├── pages/                # Non-route page components
├── routes/               # TanStack Router file-based routes
├── stores/               # Zustand stores
├── test/                 # Test setup
├── types/                # Shared type definitions
├── main.tsx              # Entry point
└── index.css             # Tailwind import
```

## Getting Started

```bash
npm install
npm run dev
```

## Scripts

| Command                 | Description               |
| ----------------------- | ------------------------- |
| `npm run dev`           | Start dev server          |
| `npm run build`         | Type check + build        |
| `npm run preview`       | Preview production build  |
| `npm run lint`          | Run ESLint                |
| `npm run lint:fix`      | Run ESLint with autofix   |
| `npm run format`        | Format with Prettier      |
| `npm run format:check`  | Check formatting          |
| `npm run typecheck`     | Run TypeScript checker    |
| `npm run test`          | Run tests (Vitest)        |
| `npm run test:watch`    | Run tests in watch mode   |
| `npm run license:check` | Audit dependency licenses |
| `make helm-docs`        | Regenerate chart README   |

## Security

- Container image runs as non-root with read-only filesystem and all capabilities dropped
- nginx serves with security headers (CSP, HSTS, X-Frame-Options, X-Content-Type-Options)
- Helm chart enforces seccomp RuntimeDefault and disallows privilege escalation
- SPDX SBOMs are attached to every published container image
- Dependency licenses are audited against an allowlist in CI

## Using This Template

After cloning, update the following for your project:

| What           | Where                            | Action                                                    |
| -------------- | -------------------------------- | --------------------------------------------------------- |
| Package name   | `package.json` → `name`          | Rename to your project                                    |
| Page title     | `index.html` → `<title>`         | Set your app name                                         |
| Favicon        | `public/vite.svg`                | Replace with your icon                                    |
| App header     | `src/components/layout/`         | Update branding                                           |
| License file   | `LICENSE`                        | Replace with your license                                 |
| License header | `hack/license-header.ts`         | Update header text, then run `npm run lint:fix` to apply  |
| Image label    | `Dockerfile` → `LABEL`           | Update `org.opencontainers.image.source` to your repo URL |
| Chart name     | `charts/application/Chart.yaml`  | Update `name` field in `Chart.yaml`                       |
| Chart image    | `charts/*/values.yaml` → `image` | Set `image.repository` to your `ghcr.io` image            |
| AI context     | `AGENTS.md`                      | Update project description for AI tools                   |

## Adding shadcn/ui Components

```bash
npx shadcn@latest init
npx shadcn@latest add button
```

## Conventions

- `@/` path alias resolves to `src/`
- File-based routing via TanStack Router (`src/routes/`)
- Query keys centralized in `src/api/query-keys.ts`
- All `.ts/.tsx` files must have the license header from `hack/license-header.ts`
- `src/routeTree.gen.ts` is auto-generated — do not edit
- Environment variables prefixed with `VITE_` (see `.env.example`)
- Pre-commit hook runs lint-staged (ESLint + Prettier on staged files)
- CI runs lint, format, typecheck, tests, license check, and build on PRs
- Tag push (`v*`) publishes container image + Helm chart to `ghcr.io`
