# Event Sourcing CQRS with Axon (Spring Boot)

A simple two-service demo showing CQRS + Event Sourcing with Axon Framework on Spring Boot:
- `cqrs` service: command + query sides, persists to PostgreSQL, publishes domain events.
- `analytics` service: consumes events to build simple account analytics (H2 in-memory) with a small chart UI.

## Architecture
- **AxonServer**: event routing and storage (via Docker Compose).
- **CQRS service (port 8787)**: REST endpoints, PostgreSQL persistence, Springdoc OpenAPI.
- **Analytics service (port 8788)**: reads events and exposes analytics + static chart page.

![Axon Overview](images/axon%20overview.png)

## Prerequisites
- JDK 17+
- Maven (or use the provided `mvnw`/`analytics/mvnw` wrappers)
- Docker (optional, for AxonServer + PostgreSQL)

## Quick Start

### Option A: Run infra via Docker Compose
Start AxonServer, PostgreSQL, and pgAdmin:

```bash
docker compose up -d
```

- AxonServer UI: http://localhost:8024
- AxonServer gRPC: 8124
- PostgreSQL: 5432 (db `accountsdb`, user `admin`, password `1234`)
- pgAdmin: http://localhost:8088 (email `med@gmail.com`, password `azer`)

### Option B: Run services locally
1) Build the CQRS service:

```bash
./mvnw clean install
```

2) Run the CQRS service (port 8787):

```bash
./mvnw spring-boot:run
```

3) In a second terminal, run Analytics (port 8788):

```bash
cd analytics
./mvnw spring-boot:run
```

## Configuration
- CQRS DB URL is configurable via env var `DB_URL`. Default:
	`jdbc:postgresql://localhost:5432/accountsdb`
- CQRS credentials: `admin` / `1234`
- Analytics uses H2 in-memory by default (no external DB needed).

## APIs & UI
- CQRS OpenAPI UI: http://localhost:8787/swagger-ui/index.html
- Analytics chart page: http://localhost:8788/chart.html

![api docs](images/api%20docs.png)
![Analytics Chart](images/chart.png)

## Repository Layout
- CQRS app: `src/main/java/com/example/cqrs` and `src/main/resources/application.properties`
- Analytics app: `analytics/src/main/java/com/example/analytics` and `analytics/src/main/resources/application.properties`
- Docker Compose: `compose.yaml`

## Notes
- Ensure Docker compose is up before starting services if you want AxonServer and PostgreSQL locally.
- If you change the DB connection, export `DB_URL` before running the CQRS service.

