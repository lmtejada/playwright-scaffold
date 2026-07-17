# Playwright Automation Scaffold

A starting point for Playwright + TypeScript test automation projects. Clone or use this repo as a GitHub template, then customize it for your application — the tooling, folder structure, and conventions are already wired up so you can start writing tests on day one.

**Stack:** Playwright · TypeScript · ESLint + Prettier · Husky + lint-staged · GitHub Actions

---

## Using this as a scaffold

To start a new project from this repo:

1. Click **"Use this template"** on GitHub (or clone it and re-init git: `rm -rf .git && git init`).
2. Update `package.json` — set `name`, `description`, and `author` for the new project.
3. Update `.env.example` with the environment variables your app actually needs, then run `cp .env.example .env.dev` and fill in real values.
4. Replace [tests/auth.setup.ts](tests/auth.setup.ts) — it currently logs into [SauceDemo](https://www.saucedemo.com) as a worked example of the storage-state pattern. Swap in your app's login flow (or delete the `setup` project in [playwright.config.ts](playwright.config.ts) if the app doesn't need pre-authenticated state).
5. Delete [tests/example.spec.ts](tests/example.spec.ts) — it's the default Playwright starter test, kept only as a smoke check that the install works.
6. Set `APP_URL` and uncomment `baseURL` in [playwright.config.ts](playwright.config.ts) if your tests should use relative URLs.
7. Update the `env` block in [.github/workflows/playwright.yml](.github/workflows/playwright.yml) to match your renamed environment variables, and configure the corresponding repo secrets/variables.
8. Start filling in `src/pages`, `src/fixtures`, `src/types`, and `src/test-data/factories` (currently placeholders) as your suite grows.

Everything else — linting, formatting, git hooks, tagging scripts, and CI — works out of the box.

---

## Getting Started

### Prerequisites

- Node.js `v22.22.3` (see `.nvmrc`; run `nvm use`)

### Setup

```bash
npm install
npx playwright install --with-deps
cp .env.example .env.dev   # fill in APP_URL, API_URL, and user/admin credentials
```

Environment files are selected by the `ENVIRONMENT` variable (defaults to `dev`, loading `.env.dev`):

```bash
ENVIRONMENT=staging npx playwright test
```

---

## Folder Structure

```
playwright-scaffold/
├── .github/
│   └── workflows/
│       └── playwright.yml      # CI: installs deps/browsers, runs the suite, uploads HTML report
├── .husky/
│   └── pre-commit               # Runs lint-staged before each commit
├── src/
│   ├── enums/
│   │   └── app.ts                # Shared enums (e.g. StorageStatePaths for auth state files)
│   ├── fixtures/                 # Custom Playwright fixtures (empty — add as needed)
│   ├── pages/                    # Page object models (empty — add as needed)
│   ├── types/                    # Shared TypeScript types (empty — add as needed)
│   ├── utils/
│   │   └── config.ts             # Env var helpers (e.g. getEnv)
│   └── test-data/
│       ├── factories/            # Dynamic test data builders (empty — add as needed)
│       └── static/
│           └── users.json        # Static test data (user personas, credentials, etc.)
├── tests/
│   ├── api/                      # API tests (empty — add as needed)
│   ├── e2e/                      # End-to-end tests (empty — add as needed)
│   ├── functional/               # Functional tests (empty — add as needed)
│   ├── auth.setup.ts             # Example login/storage-state setup — replace for your app
│   └── example.spec.ts           # Default Playwright starter test — delete once verified
├── .env.example                  # Template for required environment variables
├── .env.dev                      # Local env file (git-ignored; copy from .env.example)
├── eslint.config.mts             # Flat ESLint config (TypeScript + Playwright + Prettier rules)
├── playwright.config.ts          # Playwright projects, reporters, timeouts, storage state setup
├── tsconfig.json                 # TypeScript compiler options
└── package.json                  # Scripts and dependencies
```

---

## Available Scripts

| Command                                                  | Description                                                  |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| `npm test`                                               | Run the full suite across all configured projects            |
| `npm run test:chromium` / `test:firefox` / `test:webkit` | Run against a single browser (excludes `@destructive` tests) |
| `npm run test:ci`                                        | Single-worker Chromium run, as used in CI                    |
| `npm run test:smoke` / `test:sanity` / `test:regression` | Run tests tagged `@smoke`, `@sanity`, or `@regression`       |
| `npm run test:api` / `test:e2e`                          | Run tests tagged `@api` or `@e2e`                            |
| `npm run test:destructive`                               | Run tests tagged `@destructive` (single worker)              |
| `npm run test:debug`                                     | Run in Playwright's debug/inspector mode                     |
| `npm run test:ui`                                        | Run with Playwright's UI mode                                |
| `npm run test:headed`                                    | Run headed (excludes `@destructive` tests)                   |
| `npm run report`                                         | Open the last HTML report                                    |
| `npm run lint` / `lint:fix`                              | Lint (and auto-fix) the codebase                             |
| `npm run format`                                         | Format the codebase with Prettier                            |

Tag-based scripts rely on `@tag` annotations in test titles (e.g. `test('... @smoke', ...)`), which will be added as specs are written.

---

## Code Quality

- **ESLint** (`eslint.config.mts`) enforces TypeScript strictness and a set of Playwright best practices — no hard waits (`no-wait-for-timeout`), web-first assertions, no `test.only`/skipped tests, semantic locators over raw/nth-based ones, no `console` usage, and more.
- **Prettier** enforces consistent formatting (tabs, single quotes, 80-char width).
- **Husky + lint-staged** run ESLint and Prettier on staged files before each commit.

## CI/CD

`.github/workflows/playwright.yml` runs on every push/PR to `main`/`master`: installs dependencies and browsers, runs `npx playwright test`, and uploads the HTML report as a workflow artifact (30-day retention). Update the `env` block with your project's environment variables and configure them as repo secrets/variables before relying on it.
