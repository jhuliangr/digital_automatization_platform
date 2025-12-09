Digital Automatization Platform
===

An workflow automation platform that allows you to connect apps, services, and APIs without coding.

Architecture
===
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