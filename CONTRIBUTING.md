# Contributing

Thanks for looking at the **ERM Technical & Business Complexity Demo**. This
is a small, intentionally legacy-flavored Spring Boot/JSP prototype (see
`README.md` for the full feature tour). This guide covers build, local run,
and code style.

## Build

Requires JDK 8–17 and Maven 3.8+ (source/target is Java 8).

```bash
mvn -q package -DskipTests
```

There is no `src/test` directory yet, so `mvn test` runs zero tests — that's
expected, not a build failure.

## Run locally

```bash
mvn spring-boot:run
```

This starts the app on H2 (in-memory, default) at **http://localhost:8090**.
Wait for `Started ErmComplexityApplication`, then try `/dashboard`, `/risks`,
`/integrations`, `/security`, `/legacy`, or `/api/health`.

Optional DB profiles:

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=postgres
mvn spring-boot:run -Dspring-boot.run.profiles=oracle
```

## Code style

There is no Checkstyle/PMD/SpotBugs configured, so please match the existing
file conventions by eye:

- **Java 8 syntax only** — no `var`, no diamond operator on anonymous
  classes, no modules. Source/target is pinned to 1.8 in `pom.xml`.
- **4-space indentation**, braces on the same line as the declaration.
- **Keep pre-diamond generics** (e.g. `new HashMap<String, Object>()`) as
  used in `ApiController`, `AccessControlService`, and `RiskService` — this
  is deliberate legacy-era styling for the demo, not a bug to "modernize".
- Classes that embody specific technical debt use a Javadoc header starting
  with `LEGACY PROBLEM:` explaining the scenario being modeled (see
  `ErmComplexityApplication`). Keep using that pattern when you add or touch
  similar classes.
- Keep changes minimal and scoped — this repo intentionally mirrors a large,
  messy legacy estate at a small scale.
