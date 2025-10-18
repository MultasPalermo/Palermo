# Palermo Cart (Polyrepo Architecture)

A shopping cart application built using a polyrepo architecture, with separate repositories for each module to enable independent development, deployment, and scaling.

## Project Structure

This workspace contains five independent repositories:

| Repository | Description | Technology |
|------------|-------------|------------|
| [Palermo-cart-docs](./Palermo-cart-docs) | Documentation, architecture diagrams, and ADRs | Markdown |
| [Palermo-cart-api](./Palermo-cart-api) | Backend REST API and business logic | Golang |
| [Palermo-cart-portal](./Palermo-cart-portal) | Web frontend for end-users | React/Vue/Angular |
| [Palermo-cart-app](./Palermo-cart-app) | Mobile application for iOS and Android | Flutter/React Native |
| [Palermo-cart-db](./Palermo-cart-db) | PostgreSQL database schema and migrations | PostgreSQL |

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
git clone --recursive https://github.com/MultasPalermo/Palermo-cart.git
cd Palermo-cart

# OR if already cloned without --recursive:
git clone https://github.com/MultasPalermo/Palermo-cart.git
cd Palermo-cart
git submodule update --init --recursive
```

Each subdirectory is a Git submodule pointing to its own repository:
- `Palermo-cart-docs/` → https://github.com/MultasPalermo/Palermo-cart-docs
- `Palermo-cart-api/` → https://github.com/MultasPalermo/Palermo-cart-api
- `Palermo-cart-portal/` → https://github.com/MultasPalermo/Palermo-cart-portal
- `Palermo-cart-app/` → https://github.com/MultasPalermo/Palermo-cart-app
- `Palermo-cart-db/` → https://github.com/MultasPalermo/Palermo-cart-db

### Setup

Refer to each repository's README for detailed setup instructions:

1. **Database**: [Palermo-cart-db/README.md](./Palermo-cart-db/README.md)
2. **API**: [Palermo-cart-api/README.md](./Palermo-cart-api/README.md)
3. **Portal**: [Palermo-cart-portal/README.md](./Palermo-cart-portal/README.md)
4. **Mobile App**: [Palermo-cart-app/README.md](./Palermo-cart-app/README.md)
5. **Documentation**: [Palermo-cart-docs/README.md](./Palermo-cart-docs/README.md)

For a comprehensive getting started guide, see [Getting Started Guide](./Palermo-cart-docs/workflows/getting-started.md).

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

See [Release Process](./Palermo-cart-docs/workflows/release-process.md) for detailed instructions.

## Important Rules

- **Never commit directly** to `dev`, `qa`, `staging`, or `main`
- Each HU must have branches for each environment it passes through
- Always request **Code Review** before merging
- Keep branches **short-lived** (2-3 days maximum)
- Update **documentation** after merges to `staging` or `main`
- Each release must be **tagged** with version number

## Documentation

Comprehensive documentation is available in the [Palermo-cart-docs](./Palermo-cart-docs) repository:

- [Getting Started Guide](./Palermo-cart-docs/workflows/getting-started.md)
- [Branching Policy](./Palermo-cart-docs/workflows/branching-policy.md)
- [Release Process](./Palermo-cart-docs/workflows/release-process.md)
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
cd Palermo-cart-db
createdb palermo_cart_dev
psql -d palermo_cart_dev -f migrations/001_initial_schema.sql

# 2. Start API
cd ../Palermo-cart-api
cp .env.example .env
go run main.go

# 3. Start Web Portal
cd ../Palermo-cart-portal
npm install
npm run dev

# 4. Start Mobile App (optional)
cd ../Palermo-cart-app
flutter run
# or
npm run android
```

## Testing

Each repository contains its own test suite:

```bash
# API tests
cd Palermo-cart-api && go test ./...

# Portal tests
cd Palermo-cart-portal && npm test

# Mobile app tests
cd Palermo-cart-app && flutter test
```

## Contributing

1. Read the [Getting Started Guide](./Palermo-cart-docs/workflows/getting-started.md)
2. Follow the [Branching Policy](./Palermo-cart-docs/workflows/branching-policy.md)
3. Write tests for new features
4. Update documentation
5. Create Merge Request with detailed description

## Support

- **Documentation**: [Palermo-cart-docs](./Palermo-cart-docs)
- **Issues**: Use GitHub/GitLab Issues in respective repositories
- **Team Channel**: #palermo-cart-dev on Slack

## License

[Add your license information here]

## Team

- **Project Manager**: TBD
- **Tech Lead**: TBD
- **Dev Team**: TBD
- **QA Lead**: TBD
- **DevOps**: TBD
