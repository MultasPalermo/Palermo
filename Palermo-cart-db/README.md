# Palermo Cart Database

PostgreSQL database schema, migrations, stored procedures, and database-related scripts.

## Repository Information

- **Category**: Database
- **Technology**: PostgreSQL
- **Purpose**: Database schema, migrations, and stored procedures

## Branching Policy

### Main Branches

This repository follows a **4-environment branching strategy**:

- `dev` - Development environment database
- `qa` - Quality Assurance environment database
- `staging` - Pre-production environment database
- `main` - Production environment database

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

# Create database migrations
# Update schema or stored procedures

# Create MR: HU-01-dev → dev
```

**Closure**: Development is closed when branch `dev` is updated.

### 2. Quality Assurance (qa)

```bash
git pull origin qa
git switch qa
git switch -c HU-01-qa

# Apply QA-specific database changes
# Validate migrations and data integrity

# Create MR: HU-01-qa → qa
```

**Closure**: QA development is closed when branch `qa` is updated.

### 3. Staging

```bash
git pull origin staging
git switch staging
git switch -c HU-01-staging

# Apply staging database changes
# Integrate only approved HUs from QA

# Create MR: HU-01-staging → staging
```

**Closure**: Staging is closed when branch `staging` is updated.

### 4. Main (Production)

```bash
git pull origin main
git switch main
git switch -c release.1.1

# Consolidate all approved database changes
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
- Always test migrations on non-production environments first
- Create rollback scripts for all migrations
- Follow [Conventional Commits](https://www.conventionalcommits.org/) format:
  - `feat:` - New tables, columns, or features
  - `fix:` - Bug fixes in stored procedures or constraints
  - `chore:` - Maintenance tasks
  - `docs:` - Documentation changes
  - `refactor:` - Schema refactoring

## Code Review Process

1. Always request Code Review before merging
2. Ensure migrations are reversible
3. Test on sample data
4. Verify performance impact
5. Document changes in `Palermo-cart-docs` after merges to `staging` or `main`

## Release Management

- Each release must be tagged: `v1.x.x`
- Weekly sprint releases
- Tag format: `v{MAJOR}.{MINOR}.{PATCH}`

## Setup

```bash
# Clone repository
git clone <repository-url>

# Install PostgreSQL (if not installed)
# Follow instructions for your OS

# Create database
createdb palermo_cart

# Run migrations
psql -d palermo_cart -f migrations/001_initial_schema.sql

# Load seed data (development only)
psql -d palermo_cart -f seeds/dev_data.sql
```

## Project Structure

```
Palermo-cart-db/
├── migrations/           # Database migrations
│   ├── 001_initial_schema.sql
│   ├── 002_add_users_table.sql
│   └── ...
├── rollbacks/           # Rollback scripts for each migration
│   ├── 001_rollback.sql
│   ├── 002_rollback.sql
│   └── ...
├── procedures/          # Stored procedures
│   ├── user_procedures.sql
│   ├── cart_procedures.sql
│   └── ...
├── functions/           # Database functions
├── views/               # Database views
├── seeds/               # Seed data for development
│   ├── dev_data.sql
│   └── test_data.sql
├── docs/                # Database documentation
│   └── schema_diagram.md
└── scripts/             # Utility scripts
    ├── backup.sh
    └── restore.sh
```

## Migration Guidelines

1. **Naming Convention**: `{number}_{description}.sql`
   - Example: `003_add_payment_table.sql`

2. **Always Create Rollback Scripts**: For each migration, create a corresponding rollback
   - Migration: `003_add_payment_table.sql`
   - Rollback: `003_rollback.sql`

3. **Test Migrations**:
   ```bash
   # Apply migration
   psql -d palermo_cart -f migrations/003_add_payment_table.sql

   # Test rollback
   psql -d palermo_cart -f rollbacks/003_rollback.sql
   ```

4. **Document Schema Changes**: Update schema documentation after significant changes

## Backup and Restore

```bash
# Backup database
./scripts/backup.sh

# Restore database
./scripts/restore.sh backup_file.sql
```

## Related Repositories

- [Palermo-cart-docs](../Palermo-cart-docs) - Documentation and diagrams
- [Palermo-cart-api](../Palermo-cart-api) - Backend API (uses this database)
- [Palermo-cart-portal](../Palermo-cart-portal) - Web frontend
- [Palermo-cart-app](../Palermo-cart-app) - Mobile application
