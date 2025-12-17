## 1. Project Overview

### 1.1 Problem Summary

- What does your system do?
  - An workflow automation platform that allows you to connect apps, services, and APIs without coding.
- Who are the users?
  - Shop owners, data analysts, influencers, any person who could need an RAG, anagent or an automatization.
- What problems does it solve?
  - Not having to learn to work with langflow or langgraph to do the best automatizations available.

### 1.2 Core User Flows

Example flows:

- “user is already logged in -> user saves his api key to work with chatGpt”
- “user is already logged in -> creates a new flow -> creates an llm component -> attach his api key to it”
- “user is already logged in -> writes a prompt in the prompt area of the chatgpt component that already has user's api key within it -> user clicks the run button -> user reads the result”
- “user is already logged in -> inserts an input and an output component -> user's flow is exposed”

### 1.3 Requirements

- Functional requirements (what the system must do)

  - Login system
  - Role based permissions
  - Make teams
  - Securely store api keys
  - Make flows
  - Make agents
  - Deploy flows
  - Test agents
  - Test flows

- Non-functional requirements (scalability, performance, availability, security, observability)
  - Each service must run within a 4 GB RAM enviroment
  - Each request must go through the identity server
  - Each service must be easy detachable
  - The system must offer a user-friendly interface.
  - The system must ensure proper organization of information to allow for adequate interpretation
  - La interfaz debe tener una profundidad máxima de 2 clics para acceder a cualquier funcionalidad principal
  - The system should be a web application with a low learning curve, allowing users to adapt quickly without extensive training.
  - The system must be accessible from the main web browsers (Internet Explorer 9.0 or higher, Mozilla Firefox 5.0 or higher, Google Chrome 10.0 or higher, or any other).

## 2. Architecture (High-Level + Component-Level)

### 2.1 High-Level Architecture Diagram

[Architecture diagram](https://miro.com/app/board/uXjVGcTw6b4=/?share_link_id=872253821318)

[Architecture information](https://github.com/jhuliangr/digital_automatization_platform/blob/main/architecture.md)

## 3. API Design Package

### 3.1 API Paradigm Decision

- REST
  For comunication with databases and most of the services, because there is no need for a different kind of API.
- Websocket
  For live updates or realtime feedback

### 3.2 REST Endpoints or GraphQL Schema

Provide:

- ## /login

Req

```json
{
  "user": "johndoe@mail.com",
  "password": "........."
}
```

Res

```json
  {
    "success":"true",
    "jwt": "...",
    "user": {
      "email": "johndoe@mail.com",
      ...
    }
  }
```

- ## /signup

Req

```json
{
  "name": "John Doe",
  "email": "123@123.com",
  "birth": "17/12/1011",
  "password": "......."
}
```

Res

```json
  {
    "success":"true",
    "jwt": "...",
    "user": {
      "email": "...",
      ...
    }
  }
```

- ## /apikey/store

Req

```json
{
  "name": "Chat gpt apikey",
  "key": "sk-aiusdniausdniuahsdiuasd132..."
}
```

Res

```json
{
  "success": "true",
  "message": "Api key stored successfully"
}
```

- ## /apikey/delete/{id}

Res

```json
{
  "success": "true",
  "message": "Api key deleted successfully"
}
```

- ## /flow/create?template=blanc

Res

```json
{
  "success": "true",
  "flow":{
    "name":"blanc flow",
    "components": [],
    "edges": [],
    ...
  }
}
```

- ## /flow/delete/{id}

Res

```json
{
  "success": "true",
  "message": "Flow deleted successfully"
}
```

- ## /flow/{id}/save

Req

```json
{
  "timestamp": "31231231",
  "flow": {
    "name": "Example flow",
    "components": [
      {
        "id": "1im9-1230-9mi91-2m3x2",
        "type": "input",
        ...
      },
      {
        "id": "5io9-16p0-5si91-3y3tl",
        "type": "openai-chat",
        ...
      }
    ],
    "edges": [
      {
        "id": "2id2-52v0-5gi91-2y5f5",
        "target": "5io9-16p0-5si91-3y3tl",
        "origin": "1im9-1230-9mi91-2m3x2",
        ...
      }
    ],
    ...
  }
}
```

Res

```json
{
  "success": "true",
  "message": "Flowsaved successfully"
}
```

- ## /flow/{id}/run

Res

```json
{
  "success": "true",
  "message": "Flow ran successfully"
}
```

- ## /agent/{agentId}/start_new_conversation

Req

```json
{
  "id": "true",
  "provider_api_key": "4it0-1ty0-5mi91-2d8x3",
  "token": "as0nd9uasnd09unasd0asdniasd0n9uasd",
  ...
}
```

Res

```json
{
  "success": "true",
  "data": []
}
```

- ## /agent/{agentId}/{conversationId}/message

Req

```json
{
  "id": "true",
  "provider_api_key": "4it0-1ty0-5mi91-2d8x3",
  "token": "as0nd9uasnd09unasd0asdniasd0n9uasd",
  ...
}
```

Res

```json
{
  "success": "true",
  "data": [
    {
      "id": "n10w-w8gr-918h23-9h1928h3",
      "type": "text",
      "owner":"agent",
      "content": "Hello how can i eat mango roots?"
    },
    {

      "id": "v12r-w8gf-982bfy-91nf88h2",
      "type": "text",
      "owner":"user",
      "content": "Hello how can i eat mango roots?"
    },
    ...
  ]
}
```

### 3.3 Authentication & Authorization

- JWT
  JWTs store all the information required for authentication directly in the token itself and it's encrypted.

- Roles:
  - user
    - Can make his own flows and depending on the persmissions received from the team owner, can interact with the team's flows
  - team owner
    - Can give permissions to his team members and is the only one allowed to make team's flows public or not.
  - editor
    - Is a team member who can edit flows
  - reader
    - Is a team member who can see flows
  - consumer
    - Is who ueses the automatizations and the agents, their data is never being stored.

## 4. Data Model & Storage

### 4.1 Database Choice & Justification

Choose and justify:

- Postgres

For it's relational behaviour and because it works really good.

- Why not the alternatives?

Because there is no need for things postgres can't do at this stage or things postgres is slow doing.

### 4.2 Data Schema

[Database schema](https://miro.com/app/board/uXjVGbQ-66k=/?share_link_id=589570733651)

### 4.3 Data Access & Patterns

#### Write Patterns:

- Only backend is allowed to write data in the database for ensuring consistency, security, and facilitates auditing.
- Low latency required, but not strictly real-time.
- Atomic operations via SQL transactions.
- Write-heavy operations only during design phases, not during mass execution.
- Clear separation between configuration (writes) and execution (reads).

#### Read Patterns

It's a read-heavy system, specially in

- Dashboard and UI loading
- Displaying flows and agents
- Running publicly exposed flows
- Conversations with agents
- Web component consuming public flows

#### Caching Decisions

Redis (In-Memory Cache) for cache-aside pattern

- User permissions per team
- JWT token validation
- Rate limitation
- Trafic limitation

CDN (for Web Component)

- Served via CDN
- Hash-versioned
- No direct access to sensitive data

Benefits:

- Faster overall loading times
- Reduced backend traffic
- Isolation from the main runtime

## 5. Implementation Team Proposals

[Implementation Team Proposals](https://github.com/jhuliangr/digital_automatization_platform/blob/main/project-implementation-teams.md)
