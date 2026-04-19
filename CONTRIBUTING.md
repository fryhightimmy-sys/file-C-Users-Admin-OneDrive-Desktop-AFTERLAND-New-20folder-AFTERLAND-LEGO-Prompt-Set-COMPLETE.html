# Contributing to AFTERLAND

## 🎯 Development Standards

This repository follows strict quality gates and processes to maintain a production-grade codebase for 500,000 concurrent users.

### Prerequisites

- Node.js 22.0.0+
- pnpm 9.0.0+
- Git with GPG signing capability
- Biome (integrated, no separate install needed)

### Setup

```bash
pnpm install
```

## 📋 Commit Guidelines

### Conventional Commits

All commits MUST follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring without feature/bug changes
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Build, CI/CD, or dependency updates
- `ci`: CI/CD configuration changes

**Examples:**

```
feat(contracts): add OpenAPI spec for ticketing API
fix(api-core): resolve race condition in WebSocket handler
docs: update deployment runbook for AWS
chore(deps): bump typescript to 5.5.1
```

### GPG Signed Commits (Required)

All commits must be signed with GPG:

```bash
# Configure GPG signing
git config user.signingkey <YOUR_GPG_KEY_ID>
git config commit.gpgsign true

# Commits will now be automatically signed
```

## 🔄 Pull Request Process

1. **Create a branch** from `main`:
   ```bash
   git checkout -b feat/your-feature-name
   ```

2. **Make changes** following code standards (see below).

3. **Run validation locally**:
   ```bash
   pnpm lint        # Biome linting
   pnpm type-check  # TypeScript type checking
   pnpm test        # Unit tests with Vitest
   pnpm build       # Build all apps
   ```

4. **Push and create PR**:
   ```bash
   git push origin feat/your-feature-name
   ```

5. **Ensure all checks pass**:
   - CI pipeline (<5 min)
   - Code coverage ≥80% on `packages/contracts`
   - CODEOWNERS approval
   - All conversations resolved

## 🧪 Testing Requirements

- **Unit Tests**: Vitest with `*.test.ts` or `*.spec.ts`
- **E2E Tests**: Playwright in `apps/*/e2e/`
- **Coverage Target**: 80%+ on `packages/contracts`
- **Run tests**:
  ```bash
  pnpm test
  pnpm test:coverage
  ```

## 🎨 Code Style

### Formatting

Biome handles all formatting (no Prettier needed):

```bash
pnpm format  # Auto-format all files
```

**Rules:**
- 2-space indentation
- Double quotes for strings
- Always trailing commas (ES5)
- Max line width: 100 characters
- Semicolons always

### Linting

```bash
pnpm lint  # Run Biome linter
```

### Type Safety

TypeScript strict mode enforced:

```bash
pnpm type-check
```

## 📦 Monorepo Workspace Guidelines

### Internal Imports

Use path aliases to import from other packages:

```typescript
// ✅ Good
import { defineContract } from "@afterland/contracts";
import { Button } from "@afterland/ui";

// ❌ Bad
import { defineContract } from "../../../packages/contracts/src";
```

Path aliases defined in root `tsconfig.json`:
```json
{
  "paths": {
    "@afterland/*": ["packages/*/src"],
    "@/*": ["./src/*"]
  }
}
```

### Adding Dependencies

- **Root devDependencies** (build tools, linters, test runners):
  ```bash
  pnpm add -w -D <package>
  ```

- **Workspace package dependencies**:
  ```bash
  cd packages/my-package
  pnpm add <package>
  ```

- **Internal package references** in `package.json`:
  ```json
  {
    "dependencies": {
      "@afterland/contracts": "workspace:*"
    }
  }
  ```

## 📝 Changesets

For version bumps and changelog entries, use Changesets:

```bash
pnpm changeset
```

This creates a file in `.changeset/` documenting:
- Which packages changed
- Change type (major/minor/patch)
- Summary of changes

## 🔐 Security Checklist

- ✅ No secrets in code or commits
- ✅ Dependencies scanned via Snyk (automatic)
- ✅ Commits GPG-signed
- ✅ No direct pushes to `main` (PR required)
- ✅ CODEOWNERS approval required

## 📚 Documentation

- **ADRs** (Architecture Decision Records): `docs/ADRs/`
- **Runbooks**: `docs/RUNBOOKS.md`
- **API Docs**: Generated from OpenAPI specs

## 🚀 Deployment

For deployment guidance, see:
- Staging: `.github/workflows/deploy-staging.yml`
- Production: `.github/workflows/deploy-prod.yml`

## ❓ Questions?

Open an issue or discussion in the repository.
