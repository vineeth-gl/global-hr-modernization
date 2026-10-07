# Start, Test & Coverage Guide

This guide covers how to build, run, and exercise the ERM Technical & Business
Complexity Demo locally, plus how to verify changes before committing.

## Prerequisites

| Tool | Required | Notes |
|------|----------|-------|
| JDK 8–17 | Yes | `maven.compiler.source`/`target` is `1.8` |
| Maven 3.8+ | Yes | Builds the WAR and runs Spring Boot |
| Modern browser | Yes | Chrome / Edge / Firefox, for the JSP + AngularJS UI |
| PostgreSQL / Oracle | No | Only needed to exercise the optional `postgres`/`oracle` profiles |

## Build

```bash
mvn -q package -DskipTests
```

This compiles all sources under `src/main/java`, packages the JSP views and
static assets under `src/main/webapp` / `src/main/resources/static`, and
produces `target/erm-complexity-demo-1.0.0-SNAPSHOT.war`.

## Run

```bash
# H2 in-memory — recommended for local viewing, no external DB required
mvn spring-boot:run
```

Wait for the `Started ErmComplexityApplication` log line, then open:

| Page / API | URL |
|------------|-----|
| Dashboard | http://localhost:8090/dashboard |
| Risk Register | http://localhost:8090/risks |
| Integrations | http://localhost:8090/integrations |
| Security | http://localhost:8090/security |
| Legacy Debt | http://localhost:8090/legacy |
| Health API | http://localhost:8090/api/health |
| H2 Console | http://localhost:8090/h2-console |

The default port is **8090** (chosen because 8080 is often already in use).

### Optional database profiles

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=postgres
mvn spring-boot:run -Dspring-boot.run.profiles=oracle
```

These profiles point at `application-postgres.properties` and
`application-oracle.properties` respectively; a reachable database matching
each profile is required.

## Manual (frontend) smoke test

The frontend is server-rendered JSP with AngularJS 1.x controllers
(`src/main/resources/static/js/erm-app.js`) bound to small REST endpoints
under `/api/**` (see `ApiController`). After starting the app:

1. Load `/dashboard` and confirm the scale metrics, domain counts, and
   legacy-challenge cards render (backed by `RiskService` and
   `LegacyChallengeRegistry`).
2. Load `/risks` and confirm the seeded risk register (from
   `DemoDataLoader`) appears, and that filtering by `?domain=` narrows the
   list via `RiskService#findByDomain`.
3. Load `/integrations` and confirm the simulated connector list from
   `IntegrationHub` renders.
4. Load `/security` and confirm the RBAC/ABAC/Zero-Trust posture from
   `AccessControlService` renders.
5. Hit `/api/health` directly and confirm a `200 OK` JSON body with
   `"status": "UP"`.

## Backend testing

There is currently no `src/test` directory in this repository — there are no
automated JUnit tests to run. When adding Java changes:

- Prefer adding a focused JUnit test alongside the class you change (e.g.
  `src/test/java/com/erm/legacy/service/RiskServiceTest.java` for
  `RiskService`), using `spring-boot-starter-test` (not yet a declared
  dependency — add it to `pom.xml` under `<dependencies>` with
  `<scope>test</scope>` the first time it's needed).
- Run tests with:

  ```bash
  mvn test
  ```

## Lint / static analysis

No Checkstyle, PMD, SpotBugs, or ESLint configuration is present in this
repository (`pom.xml` only declares the `spring-boot-maven-plugin`; there is
no `.eslintrc*` for the single JS file). Code style is kept consistent by
convention and manual review instead of an automated linter. If a linter is
introduced later, document the command and ruleset here.

## Related docs

- [`COMPLEXITY_ISSUES.md`](COMPLEXITY_ISSUES.md) — maps each
  Technical & Business Complexity-table category to where it is represented
  in this codebase.
