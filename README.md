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

## Guía práctica: cómo trabajar con esta arquitectura y ramas

Esta guía te lleva paso a paso por el flujo típico con polyrepo y submódulos, usando las ramas de entorno: `dev` → `qa` → `staging` → `main`.

### Conceptos clave

- Polyrepo: cada módulo (API, Portal, App, DB, Docs) está en su propio repositorio.
- Submódulos: el repo raíz solo apunta a commits específicos de cada módulo. Siempre que cambies algo en un submódulo, debes actualizar el puntero del superproyecto (repo raíz).
- Ramas de entorno: cada repo tiene `dev`, `qa`, `staging` y `main`. Trabajamos por HU (Historia de Usuario) creando ramas por entorno.

### 0) Preparación inicial (una vez)

```bash
# Clonar con submódulos
git clone --recursive https://github.com/MultasPalermo/Palermo.git
cd Palermo

# Si ya clonaste sin submódulos
git submodule update --init --recursive
```

Recomendado tras cada cambio remoto:

```bash
# Actualiza todos los submódulos a sus commits configurados
git submodule update --recursive
```

### 1) Desarrollo en `dev` (por submódulo afectado)

Ejemplo con `Palermo-api` (repite para los módulos que necesites):

```bash
cd Palermo-api
git fetch --all
git switch dev
git pull origin dev

# Crea rama de HU en dev
git switch -c HU-01-dev

# Desarrolla
# ... cambios de código ...
git add .
git commit -m "feat(api): HU-01 implementar X"

# Publica y crea PR a dev
git push -u origin HU-01-dev
# Abre un Pull Request: HU-01-dev → dev (en el repo del submódulo)
```

Tras aprobar y mergear el PR en el submódulo a `dev`, actualiza el superproyecto para apuntar a ese commit:

```bash
cd ..  # vuelve al repo raíz
git add Palermo-api
git commit -m "chore(submodules): actualizar Palermo-api a dev (HU-01)"
git push
```

Nota: si el repo raíz también usa ramas de entorno (recomendado), realiza el commit anterior sobre la rama `dev` del repo raíz.

### 2) Promoción a `qa`

Hay dos formas comunes. Para quienes comienzan, recomendamos la opción A (cherry-pick) porque trae solo los cambios de la HU.

Opción A — Cherry-pick a una rama `HU-01-qa`:

```bash
cd Palermo-api
git fetch --all
git switch qa
git pull origin qa

# Crea la rama de HU para QA
git switch -c HU-01-qa

# Trae los commits de la HU desde dev
git log --oneline origin/HU-01-dev  # identifica los SHAs de la HU
git cherry-pick <sha-inicial>^..<sha-final>

# Resuelve conflictos si aparecen, luego
git push -u origin HU-01-qa
# Crea PR: HU-01-qa → qa
```

Opción B — PR entre ramas de entorno (más simple, pero puede arrastrar cambios no deseados):

```bash
# En GitHub: abrir PR de dev → qa con el alcance de la HU
# (Úsalo solo si dev no contiene otros cambios que no deban ir a qa)
```

Tras mergear en `qa`, actualiza el puntero del superproyecto:

```bash
cd ..
git add Palermo-api
git commit -m "chore(submodules): actualizar Palermo-api a qa (HU-01)"
git push
```

### 3) Promoción a `staging`

Repite el patrón de QA:

```bash
cd Palermo-api
git fetch --all
git switch staging
git pull origin staging
git switch -c HU-01-staging

# Cherry-pick de los commits aprobados en qa
git log --oneline origin/qa
git cherry-pick <sha-inicial>^..<sha-final>

git push -u origin HU-01-staging
# PR: HU-01-staging → staging

cd ..
git add Palermo-api
git commit -m "chore(submodules): actualizar Palermo-api a staging (HU-01)"
git push
```

### 4) Preparar release a `main`

Cuando el sprint está listo para producción:

```bash
cd Palermo-api
git fetch --all
git switch main
git pull origin main

# O crea/usa rama de release si aplica
git switch -c release.1.2  # ejemplo

# Cherry-pick desde staging (o merge si el repositorio lo permite)
git log --oneline origin/staging
git cherry-pick <sha-inicial>^..<sha-final>

git push -u origin release.1.2
# PR: release.1.2 → main

cd ..
git add Palermo-api
git commit -m "chore(submodules): actualizar Palermo-api para release 1.2"
git push
```

Tras el merge a `main`, etiqueta el release en el submódulo:

```bash
cd Palermo-api
git switch main
git pull
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```

### Mantener submódulos sincronizados

- Después de merges en submódulos, siempre realiza un commit en el repo raíz actualizando el puntero del submódulo.
- Si el repo raíz maneja ramas por entorno, repite la actualización en la rama correspondiente (`dev`, `qa`, `staging`, `main`).
- Para traer la última referencia del submódulo en una rama concreta:

```bash
cd Palermo-api
git fetch
git switch dev  # o qa/staging/main
git pull
cd ..
git add Palermo-api
git commit -m "chore(submodules): actualizar puntero de Palermo-api"
git push
```

### Errores comunes y cómo evitarlos

- Olvidar actualizar el repo raíz tras mergear en un submódulo → Solución: `git add <submodulo> && git commit && git push` en el repo raíz.
- Crear PRs entre ramas de entorno que arrastran cambios no deseados → Prefiere cherry-pick por HU.
- Conflictos de cherry-pick → Resuelve conflictos archivo por archivo, valida tests y continúa con `git cherry-pick --continue`.
- Trabajar en la rama de entorno directamente (`dev`, `qa`, `staging`, `main`) → Siempre usa ramas `HU-*` y PRs.

### Checklist rápida por HU

1) Rama `HU-xx-dev` → PR a `dev` (submódulo)

2) Actualiza repo raíz (puntero del submódulo)

3) Rama `HU-xx-qa` (desde `qa`) + cherry-pick → PR a `qa`

4) Actualiza repo raíz

5) Rama `HU-xx-staging` (desde `staging`) + cherry-pick → PR a `staging`

6) Actualiza repo raíz

7) Rama de `release.X.Y` (o directo a `main` según política) → PR a `main` + tag

8) Actualiza repo raíz (puntero final)
