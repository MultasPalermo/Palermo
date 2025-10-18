# Palermo  (Polyrepo Architecture)

A shopping  application built using a polyrepo architecture, with separate repositories for each module to enable independent development, deployment, and scaling.

## Project Structure

This workspace contains five independent repositories:

| Repository | Description | Technology |
|------------|-------------|------------|
| [Palermo--docs](./Palermo--docs) | Documentation, architecture diagrams, and ADRs | Markdown |
| [Palermo--api](./Palermo--api) | Backend REST API and business logic | Golang |
| [Palermo--portal](./Palermo--portal) | Web frontend for end-users | React/Vue/Angular |
| [Palermo--app](./Palermo--app) | Mobile application for iOS and Android | Flutter/React Native |
| [Palermo--db](./Palermo--db) | PostgreSQL database schema and migrations | PostgreSQL |

## Quick Start

### Prerequisites

- Git 2.30+
- Go 1.21+ (for API)
- Node.js 18+ (for Portal/App)
- PostgreSQL 14+ (for Database)
- Flutter SDK or React Native CLI (for Mobile)

### Clone Workspace with Submodules

This workspace uses **Git Submodules**. To clone with all repositories:

```bash
# Clone the workspace repository with all submodules
git clone --recursive https://github.com/MultasPalermo/Palermo-.git
cd Palermo-

# OR if already cloned without --recursive:
git clone https://github.com/MultasPalermo/Palermo-.git
cd Palermo-
git submodule update --init --recursive
```

Each subdirectory is a Git submodule pointing to its own repository:
- `Palermo--docs/` → https://github.com/MultasPalermo/Palermo-docs
- `Palermo--api/` → https://github.com/MultasPalermo/Palermo-api
- `Palermo--portal/` → https://github.com/MultasPalermo/Palermo-portal
- `Palermo--app/` → https://github.com/MultasPalermo/Palermo-app
- `Palermo--db/` → https://github.com/MultasPalermo/Palermo-db

### Setup

Refer to each repository's README for detailed setup instructions:

1. **Database**: [Palermo--db/README.md](./Palermo-db/README.md)
2. **API**: [Palermo--api/README.md](./Palermo-api/README.md)
3. **Portal**: [Palermo--portal/README.md](./Palermo-portal/README.md)
4. **Mobile App**: [Palermo--app/README.md](./Palermo-app/README.md)
5. **Documentation**: [Palermo--docs/README.md](./Palermo-docs/README.md)

For a comprehensive getting started guide, see [Getting Started Guide](./Palermo--docs/workflows/getting-started.md).

## Branching Strategy

All repositories follow a **4-environment branching strategy**:

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

### Release Branches

Only `main` uses release branches for production deployments:

```
release.{SPRINT}.{VERSION}
```

Example: `release.1.1`, `release.1.2`

## Development Workflow

### 1. Create Feature Branch

```bash
# Start from dev
git switch dev
git pull origin dev
git switch -c HU-01-dev
```

### 2. Develop and Commit

```bash
# Make changes
git add .
git commit -m "feat: add user authentication"
```

### 3. Push and Create MR

```bash
git push origin HU-01-dev
# Create Merge Request: HU-01-dev → dev
```

### 4. Progress Through Environments

After approval in each environment:
- `dev` → Create `HU-01-qa`
- `qa` → Create `HU-01-staging`
- `staging` → Include in `release.X.Y`

## Commit Convention

Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `chore:` - Maintenance tasks
- `refactor:` - Code refactoring
- `test:` - Test additions or modifications

Example:
```bash
git commit -m "feat(auth): implement JWT token validation

Add middleware for JWT token validation including
expiration check and user role verification.

Closes #123"
```

## Release Management

- **Frequency**: Weekly sprint releases (every Friday)
- **Versioning**: Semantic Versioning (MAJOR.MINOR.PATCH)
- **Tags**: All releases must be tagged (e.g., `v1.2.0`)

See [Release Process](./Palermo--docs/workflows/release-process.md) for detailed instructions.

## Important Rules

- **Never commit directly** to `dev`, `qa`, `staging`, or `main`
- Each HU must have branches for each environment it passes through
- Always request **Code Review** before merging
- Keep branches **short-lived** (2-3 days maximum)
- Update **documentation** after merges to `staging` or `main`
- Each release must be **tagged** with version number

## Documentation

Comprehensive documentation is available in the [Palermo--docs](./Palermo--docs) repository:

- [Getting Started Guide](./Palermo--docs/workflows/getting-started.md)
- [Branching Policy](./Palermo--docs/workflows/branching-policy.md)
- [Release Process](./Palermo--docs/workflows/release-process.md)
- Architecture Diagrams (coming soon)
- API Documentation (coming soon)

## Architecture

This project uses a **polyrepo architecture** (multiple repositories) as opposed to a monorepo (single repository). Benefits include:

- **Independent deployment** of each module
- **Isolated version control** and release cycles
- **Clear ownership** and boundaries
- **Flexible technology** choices per module
- **Reduced blast radius** for changes

## Development Setup

### Quick Start (All Services)

```bash
# 1. Set up database
cd Palermo--db
createdb palermo__dev
psql -d palermo__dev -f migrations/001_initial_schema.sql

# 2. Start API
cd ../Palermo--api
cp .env.example .env
go run main.go

# 3. Start Web Portal
cd ../Palermo--portal
npm install
npm run dev

# 4. Start Mobile App (optional)
cd ../Palermo--app
flutter run
# or
npm run android
```

## Testing

Each repository contains its own test suite:

```bash
# API tests
cd Palermo--api && go test ./...

# Portal tests
cd Palermo--portal && npm test

# Mobile app tests
cd Palermo--app && flutter test
```

## Contributing

1. Read the [Getting Started Guide](./Palermo--docs/workflows/getting-started.md)
2. Follow the [Branching Policy](./Palermo--docs/workflows/branching-policy.md)
3. Write tests for new features
4. Update documentation
5. Create Merge Request with detailed description

## Support

- **Documentation**: [Palermo--docs](./Palermo--docs)
- **Issues**: Use GitHub/GitLab Issues in respective repositories
- **Team Channel**: #palermo--dev on Slack

## License

[Add your license information here]

## Team

- **Project Manager**: TBD
- **Tech Lead**: TBD
- **Dev Team**: TBD
- **QA Lead**: TBD
- **DevOps**: TBD
