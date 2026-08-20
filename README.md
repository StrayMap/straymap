# StrayMap

Community-driven platform for mapping stray cat colonies, feeding points, and coordinating care between reporters, volunteers, and animal welfare organizations.

StrayMap lets anyone report a cat sighting or feeding point on a map. Organizations (NGOs, vets) take Spots under their care, log interventions (sterilization, vaccination), and coordinate coverage with other organizations — without exposing exact locations publicly.

## Repositories

| Repo | Stack | Description |
|---|---|---|
| [straymap-backend](https://github.com/straymap/straymap-backend) | Java / Spring Boot / PostgreSQL+PostGIS | REST API, geometry, domain logic |
| [straymap-web](https://github.com/straymap/straymap-web) | React / TypeScript / Leaflet | Web app |
| [straymap-mobile](https://github.com/straymap/straymap-mobile) | Flutter | iOS + Android app |

This repo holds no code — only documentation, architecture decisions, and links.

## Architecture

- **Backend**: hexagonal (ports & adapters) + lightweight DDD, organized by domain module (`spot/`, `cat/`, `organization/`, `geo/`, ...)
- **Frontend**: two independent codebases (web and mobile), no shared code — deliberate choice for learning both ecosystems (see ADR-006)
- **Database**: PostgreSQL with PostGIS for spatial queries
- **File storage**: MinIO (S3-compatible)

Full design discussion, entity model, and diagrams: [Miro board](https://miro.com/app/board/uXjVHxeRCV8=/)

## Key design decisions (ADRs)

Architecture Decision Records live in [Confluence](https://straymap.atlassian.net/wiki/spaces/SD/overview):

- **ADR-001** — Published geometry always comes from a fixed set of shapes (grid cells), never from freeform data
- **ADR-002** — `Spot` as a unified entity (colony / solitary cat / feeding point)
- **ADR-003** — Organization coverage areas are derived from Spots, not drawn
- **ADR-004** — No moderation at launch; post-hoc cleanup instead
- **ADR-005** — Cat status (`ACTIVE`/`MISSING`/`DECEASED`) separate from observed condition
- **ADR-006** — Flutter for mobile, separate React app for web

## Project tracking

Issues and sprint work are tracked in [Jira (SM project)](https://straymap.atlassian.net/jira/software/projects/SM/boards/1).

## Privacy principle

Exact coordinates are never exposed publicly. Public map data is aggregated to grid cells (250m for Spots, 500m for organization coverage areas). Exact locations are visible only to the reporter (for their own reports) and validated volunteers/organizations. See ADR-001 for details.
