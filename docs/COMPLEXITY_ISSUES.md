# Complexity-Table Category → Issue Mapping

This demo intentionally represents each category of a "15+ year hybrid
monolith/microservices estate" Technical & Business Complexity table in a
scaled-down (`< 5,000` LOC) codebase. This document maps each category to the
concrete code, data, or UI surface that demonstrates it.

| Category | Represented by | Key files |
|----------|-----------------|-----------|
| Multiple acquired products | Seeded risk IDs prefixed `RG-` (RiskGuard), `AP-` (AuditPro), `VS-` (VendorSight) | `DemoDataLoader`, `LegacyDebtCatalog#ACQUIRED_PRODUCTS` |
| Hybrid monolithic + microservices architecture | Single Spring MVC monolith WAR standing in for a split monolith/sidecar estate | `ErmComplexityApplication`, `LegacyDebtCatalog#HYBRID_ARCHITECTURE` |
| Shared business services | A facade used by controllers, batch jobs, and services alike | `SharedBusinessServices`, `LegacyDebtCatalog#SHARED_BUSINESS_SERVICES` |
| High code dependencies | JSP views including AngularJS partials that call back into Spring MVC controllers | `src/main/webapp/WEB-INF/jsp/*.jsp`, `erm-app.js`, `LegacyDebtCatalog#HIGH_DEPENDENCIES` |
| Multiple deployment models | WAR packaging (`pom.xml`), `SpringBootServletInitializer`, and notes on historical EAR/WebLogic deployment | `ErmComplexityApplication`, `LegacyDebtCatalog#MULTIPLE_DEPLOYMENTS` |
| Legacy frameworks | Java 8 target, Spring MVC, JSP/JSTL, AngularJS 1.8 | `pom.xml`, `WebMvcConfig`, `erm-app.js`, `LegacyDebtCatalog#LEGACY_FRAMEWORKS` |
| 9 legacy risk domains | `RiskDomain` enum covering ERM, Operational, Audit, Policy, Compliance, IT, Cyber, Third-Party, Vendor risk | `RiskDomain` |
| Dual-database reality | Oracle vs PostgreSQL dialect resolution by region, plus three Spring profiles (H2 default, Postgres, Oracle) | `SharedBusinessServices#resolveTenantDialect`, `application*.properties` |
| Integration sprawl | Nine simulated connectors across ERP/ITSM/IAM/Security/HR categories, with one shown `DEGRADED` | `IntegrationHub` |
| Security posture (Zero Trust, MFA, RBAC/ABAC, encryption) | RBAC permission matrix plus an ABAC attribute gate, combined into a dashboard-ready posture map | `AccessControlService` |
| Scale narrative (LOC, modules, APIs, tables, frameworks, regions) | A single DTO summarising the demo's scaled-down numbers next to the production-estate equivalents | `ScaleMetrics`, `RiskService#scaleMetrics` |

## How these surface in the UI

- `/dashboard` — scale metrics, domain counts, and the legacy-challenge list.
- `/risks` — the seeded risk register with acquired-product ID prefixes,
  filterable by domain.
- `/integrations` — the simulated connector list, including the one
  `DEGRADED` entry.
- `/security` — the Zero Trust / MFA / RBAC / ABAC posture surface.
- `/legacy` — the explicit legacy-challenge catalog and per-region dialect
  notes (`US-EAST` → Oracle, `EU-WEST` → PostgreSQL).

See [`START_TEST_AND_ISSUES.md`](START_TEST_AND_ISSUES.md) for how to build,
run, and manually verify each of these pages.
