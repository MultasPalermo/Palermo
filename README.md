# Palermo (Arquitectura Polyrepo)

Aplicación de compras construida con una arquitectura polyrepo, con repositorios separados por módulo para permitir desarrollo, despliegue y escalado de forma independiente.

## Estructura del proyecto

Este workspace contiene cinco repositorios independientes:

| Repositorio | Descripción | Tecnología |
|-------------|-------------|------------|
| [Palermo-docs](./Palermo-docs) | Documentación, diagramas de arquitectura y ADRs | Markdown |
| [Palermo-api](./Palermo-api) | API REST de backend y lógica de negocio | Golang |
| [Palermo-portal](./Palermo-portal) | Frontend web para usuarios finales | React/Vue/Angular |
| [Palermo-app](./Palermo-app) | Aplicación móvil para iOS y Android | Flutter/React Native |
| [Palermo-db](./Palermo-db) | Esquema de base de datos y migraciones | PostgreSQL |

## Inicio rápido

### Requisitos previos

- Git 2.30+
- Go 1.21+ (para la API)
- Node.js 18+ (para Portal/App)
- PostgreSQL 14+ (para la base de datos)
- Flutter SDK o React Native CLI (para Mobile)

### Clonar el workspace con submódulos

Este workspace usa **Git Submodules**. Para clonar con todos los repositorios:

```bash
# Clonar el repositorio del workspace con todos los submódulos
git clone --recursive https://github.com/MultasPalermo/Palermo.git
cd Palermo

# O si ya lo clonaste sin --recursive
git clone https://github.com/MultasPalermo/Palermo.git
cd Palermo
git submodule update --init --recursive
```

Cada subcarpeta es un submódulo que apunta a su propio repositorio:
- `Palermo-docs/` → https://github.com/MultasPalermo/Palermo-docs.git
- `Palermo-api/` → https://github.com/MultasPalermo/Palermo-api.git
- `Palermo-portal/` → https://github.com/MultasPalermo/Palermo-portal.git
- `Palermo-app/` → https://github.com/MultasPalermo/Palermo-app.git
- `Palermo-db/` → https://github.com/MultasPalermo/Palermo-db.git

### Configuración

Consulta el README de cada repositorio para instrucciones detalladas:

1. Base de datos: [Palermo-db/README.md](./Palermo-db/README.md)
2. API: [Palermo-api/README.md](./Palermo-api/README.md)
3. Portal Web: [Palermo-portal/README.md](./Palermo-portal/README.md)
4. App Móvil: [Palermo-app/README.md](./Palermo-app/README.md)
5. Documentación: [Palermo-docs/README.md](./Palermo-docs/README.md)

Para una guía completa de inicio, ver [Guía de Inicio](./Palermo-docs/workflows/getting-started.md).

## Estrategia de ramas

Todos los repositorios siguen una estrategia de **4 entornos**:

- `dev` - Desarrollo
- `qa` - Aseguramiento de Calidad
- `staging` - Preproducción
- `main` - Producción

### Convención de nombres de ramas (HU)

Cada Historia de Usuario (HU) debe tener su propia rama por entorno:

```
HU-{NUMERO}-{ENTORNO}
```

Ejemplos:
- `HU-01-dev`
- `HU-01-qa`
- `HU-01-staging`

### Ramas de release

Solo `main` utiliza ramas de release para despliegues a producción:

```
release.{SPRINT}.{VERSION}
```

Ejemplos: `release.1.1`, `release.1.2`

## Flujo de desarrollo

### 1. Crear rama de funcionalidad

```bash
# Partiendo desde dev
git switch dev
git pull origin dev
git switch -c HU-01-dev
```

### 2. Desarrollar y commitear

```bash
# Realiza cambios
git add .
git commit -m "feat: agregar autenticación de usuarios"
```

### 3. Push y creación de PR

```bash
git push origin HU-01-dev
# Crear Pull Request: HU-01-dev → dev
```

### 4. Progresión por entornos

Tras la aprobación en cada entorno:
- De `dev` → crear `HU-01-qa`
- De `qa` → crear `HU-01-staging`
- De `staging` → incluir en `release.X.Y`

## Convención de commits

Sigue [Conventional Commits](https://www.conventionalcommits.org/lang/es/):

- `feat:` - Nueva funcionalidad
- `fix:` - Corrección de bug
- `docs:` - Cambios en documentación
- `chore:` - Tareas de mantenimiento
- `refactor:` - Refactorización de código
- `test:` - Adición o modificación de tests

Ejemplo:
```bash
git commit -m "feat(auth): implementar validación de token JWT

Agregar middleware para validar JWT incluyendo
verificación de expiración y roles de usuario.

Closes #123"
```

## Gestión de releases

- Frecuencia: releases por sprint (viernes)
- Versionado: Semantic Versioning (MAJOR.MINOR.PATCH)
- Tags: todos los releases deben ser etiquetados (por ejemplo, `v1.2.0`)

Consulta el [Proceso de Release](./Palermo-docs/workflows/release-process.md) para instrucciones detalladas.

## Reglas importantes

- No commitear directamente a `dev`, `qa`, `staging` o `main`
- Cada HU debe tener ramas para cada entorno por el que pasa
- Siempre solicitar **Code Review** antes de hacer merge
- Mantener ramas **de corta vida** (2-3 días máximo)
- Actualizar **documentación** tras merges a `staging` o `main`
- Cada release debe estar **taggeado** con número de versión

## Documentación

La documentación completa está en el repositorio [Palermo-docs](./Palermo-docs):

- [Guía de Inicio](./Palermo-docs/workflows/getting-started.md)
- [Política de Ramas](./Palermo-docs/workflows/branching-policy.md)
- [Proceso de Release](./Palermo-docs/workflows/release-process.md)
- Diagramas de arquitectura (próximamente)
- Documentación de API (próximamente)

## Arquitectura

Este proyecto utiliza una **arquitectura polyrepo** (múltiples repositorios) en lugar de monorepo (un único repositorio). Beneficios:

- **Despliegue independiente** por módulo
- **Ciclos de versionado** y releases aislados
- **Propiedad y límites claros** por equipo/módulo
- **Tecnologías flexibles** por módulo
- **Menor impacto** ante cambios

## Entorno de desarrollo

### Inicio rápido (todos los servicios)

```bash
# 1. Base de datos
cd Palermo-db
createdb palermo__dev
psql -d palermo__dev -f migrations/001_initial_schema.sql

# 2. API
cd ../Palermo-api
cp .env.example .env
go run main.go

# 3. Portal Web
cd ../Palermo-portal
npm install
npm run dev

# 4. App Móvil (opcional)
cd ../Palermo-app
flutter run
# o
npm run android
```

## Tests

Cada repositorio contiene su propia batería de tests:

```bash
# Tests de la API
cd Palermo-api && go test ./...

# Tests del Portal
cd Palermo-portal && npm test

# Tests de la App Móvil
cd Palermo-app && flutter test
```

## Contribuir

1. Lee la [Guía de Inicio](./Palermo-docs/workflows/getting-started.md)
2. Sigue la [Política de Ramas](./Palermo-docs/workflows/branching-policy.md)
3. Escribe tests para nuevas funcionalidades
4. Actualiza la documentación
5. Crea un Pull Request con descripción detallada

## Soporte

- Documentación: [Palermo-docs](./Palermo-docs)
- Issues: Usa GitHub Issues en cada repositorio
- Canal del equipo: #palermo-dev en Slack

## Licencia

[Agrega aquí la información de la licencia]

## Equipo

- Project Manager: TBD
- Tech Lead: TBD
- Dev Team: TBD
- QA Lead: TBD
- DevOps: TBD
