# Palermo Cart Portal

Web frontend interface for end-users. Built with modern web technologies.

## Repository Information

- **Category**: Frontend (Web)
- **Technology**: React/Vue.js/Angular (to be defined)
- **Purpose**: Web interface for end-users

## Branching Policy

### Main Branches

This repository follows a **4-environment branching strategy**:

- `dev` - Development environment
- `qa` - Quality Assurance environment
- `staging` - Pre-production environment
- `main` - Production environment

### Branch Naming Convention

Each User Story (HU) must have its own branch per environment:

```
HU-{NUMBER}-{ENVIRONMENT}
```

Examples:
- `HU-01-dev`
- `HU-01-qa`
- `HU-01-staging`

### Release Branches (Main Only)

Production releases use sprint-based release branches:

```
release.{SPRINT}.{VERSION}
```

Example: `release.1.1`, `release.1.2`

## Workflow by Environment

### 1. Development (dev)

```bash
git pull origin dev
git switch dev
git switch -c HU-01-dev

# Perform development work
# Implement UI features or fixes

# Create MR: HU-01-dev → dev
```

**Closure**: Development is closed when branch `dev` is updated.

### 2. Quality Assurance (qa)

```bash
git pull origin qa
git switch qa
git switch -c HU-01-qa

# Perform QA-specific development
# Validate UI tests and accessibility

# Create MR: HU-01-qa → qa
```

**Closure**: QA development is closed when branch `qa` is updated.

### 3. Staging

```bash
git pull origin staging
git switch staging
git switch -c HU-01-staging

# Perform staging-specific development
# Integrate only approved HUs from QA

# Create MR: HU-01-staging → staging
```

**Closure**: Staging is closed when branch `staging` is updated.

### 4. Main (Production)

```bash
git pull origin main
git switch main
git switch -c release.1.1

# Consolidate all approved HUs for production
# Example HUs included:
# - HU-01-release.1.1
# - HU-02-release.1.1
# - HU-03-release.1.1
# - HU-07-release.1.1
# - HU-09-release.1.1

# Create MR: release.1.1 → main
```

**Note**: One release per sprint (weekly).

## Important Rules

- **Never commit directly** to `dev`, `qa`, `staging`, or `main`
- Each HU must have branches for each environment it passes through
- Only `main` uses release branches
- Follow [Conventional Commits](https://www.conventionalcommits.org/) format:
  - `feat:` - New feature
  - `fix:` - Bug fix
  - `style:` - UI/styling changes
  - `chore:` - Maintenance tasks
  - `docs:` - Documentation changes
  - `refactor:` - Code refactoring
  - `test:` - Test additions or modifications

## Code Review Process

1. Always request Code Review before merging
2. Ensure all tests pass
3. Verify UI/UX consistency
4. Document changes in `Palermo-cart-docs` after merges to `staging` or `main`

## Release Management

- Each release must be tagged: `v1.x.x`
- Weekly sprint releases
- Tag format: `v{MAJOR}.{MINOR}.{PATCH}`

## Setup

```bash
# Clone repository
git clone <repository-url>

# Install dependencies
npm install
# or
yarn install

# Run development server
npm run dev
# or
yarn dev

# Run tests
npm test
# or
yarn test

# Build for production
npm run build
# or
yarn build
```

## Project Structure

```
Palermo-cart-portal/
├── src/
│   ├── components/    # Reusable UI components
│   ├── pages/         # Page components
│   ├── services/      # API services
│   ├── hooks/         # Custom React hooks
│   ├── utils/         # Utility functions
│   └── assets/        # Static assets
├── public/            # Public assets
├── tests/             # Test files
└── config/            # Configuration files
```

## Related Repositories

- [Palermo-cart-docs](../Palermo-cart-docs) - Documentation and diagrams
- [Palermo-cart-api](../Palermo-cart-api) - Backend API
- [Palermo-cart-app](../Palermo-cart-app) - Mobile application
- [Palermo-cart-db](../Palermo-cart-db) - Database schema
