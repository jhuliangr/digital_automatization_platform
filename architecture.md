# Digital Automatization Platform

## MVP:

An workflow automation platform that allows you to connect apps, services, and APIs without coding.

# Architecture

## Microservices:

- Identity server
- Ids database
- users and teams database
- Data backend
- python backend (websocket service)
- rabbit MQ node
- Realtime server
- Workflows exposer

## Client server:

- Webapp
- WebComponent

## Architectural patterns?

- Microservices
- Client server

## Communication

- http
- websocket

## AuthN and AuthZ

- role based
- email auth
- oauth

### Communication Between Components:

#### HTTP:

- WebApp -> Identity Server (login, token refresh)
- WebApp -> Users & Teams Service
- WebApp -> Data Backend
- WebComponent -> Identity Server / Data Backend

#### Websocket:

- Realtime Server (live updates) -> WebApp
- Python Backend (WS) -> Realtime Server (streaming computational data)
- Realtime Server (if embedded UI needs real-time data) -> WebComponent

#### Message Queue (RabbitMQ)

- Data Backend → RabbitMQ → Python service

### Authentication & Authorization: Identity server:

#### AuthN:

- Email/password
- OAuth (Google/Microsoft/Facebook)

#### AuthZ:

- Issues JWT tokens containing roles and claims

# Phase 1:

Making the product ready for real users and operation: production security, reliability

- Api Gateway
  - Unifies routes, TLS, rate limiting, basic WAF, API versioning
- Ids database backup
- Db backup
- Overservability service
  - Instrumentation for measuring the use of workflows and jobs

# Phase 2:

Making the product ready for ease of deployment, observability, scalability support, and basic administration experience.

- Kubernetes cluster
  - Scaling, deployment, rollout, environment configuration
- Service mesh
  - Service-to-service security and observability (distributed traces)
- CI/CD
  - Reproducible deployment, testing, linting, builds, and automated releases
- Secrets manager

[Phases diagram](https://miro.com/app/board/uXjVGcVq3yg=/?share_link_id=500361743487)
