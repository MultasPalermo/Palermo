# Palermo Cart App

Mobile application built with Flutter/React Native for iOS and Android platforms.

## Repository Information

- **Category**: Frontend (Mobile)
- **Technology**: Flutter/React Native (to be defined)
- **Purpose**: Mobile application for end-users
- **Platforms**: iOS and Android

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
# Implement mobile features or fixes

# Create MR: HU-01-dev → dev
```

**Closure**: Development is closed when branch `dev` is updated.

### 2. Quality Assurance (qa)

```bash
git pull origin qa
git switch qa
git switch -c HU-01-qa

# Perform QA-specific development
# Validate mobile tests and functionality

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
3. Test on both iOS and Android
4. Verify UI/UX consistency across platforms
5. Document changes in `Palermo-cart-docs` after merges to `staging` or `main`

## Release Management

- Each release must be tagged: `v1.x.x`
- Weekly sprint releases
- Tag format: `v{MAJOR}.{MINOR}.{PATCH}`

## Setup

### Flutter

```bash
# Clone repository
git clone <repository-url>

# Install dependencies
flutter pub get

# Run on device/emulator
flutter run

# Run tests
flutter test

# Build for production
flutter build apk        # Android
flutter build ios        # iOS
```

### React Native

```bash
# Clone repository
git clone <repository-url>

# Install dependencies
npm install
# or
yarn install

# Run on iOS
npm run ios
# or
yarn ios

# Run on Android
npm run android
# or
yarn android

# Run tests
npm test
# or
yarn test
```

## Project Structure

### Flutter
```
Palermo-cart-app/
├── lib/
│   ├── screens/       # Screen widgets
│   ├── widgets/       # Reusable widgets
│   ├── services/      # API services
│   ├── models/        # Data models
│   ├── providers/     # State management
│   └── utils/         # Utility functions
├── test/              # Test files
├── android/           # Android native code
├── ios/               # iOS native code
└── assets/            # Static assets
```

### React Native
```
Palermo-cart-app/
├── src/
│   ├── screens/       # Screen components
│   ├── components/    # Reusable components
│   ├── services/      # API services
│   ├── hooks/         # Custom hooks
│   ├── navigation/    # Navigation setup
│   └── utils/         # Utility functions
├── android/           # Android native code
├── ios/               # iOS native code
└── assets/            # Static assets
```

## Related Repositories

- [Palermo-cart-docs](../Palermo-cart-docs) - Documentation and diagrams
- [Palermo-cart-api](../Palermo-cart-api) - Backend API
- [Palermo-cart-portal](../Palermo-cart-portal) - Web frontend
- [Palermo-cart-db](../Palermo-cart-db) - Database schema
