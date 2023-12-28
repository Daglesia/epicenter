# Title
Preemptive decision about backend solution used
## Status
Decided to use FastAPI.
## Context

This repository is going to serve as a monolith backend for:
- Auth9, a custom service for authorization and authetication of daglesia-related services (see all below)
- HackyDucky, a service allowing booting scripts remotely to automate various game chores
- Onion, service for aggregation of coupons, loyalty cards and promotion emails
- Mirage, banking app to track expenses and possibly help savings

And possibly more.

Each of these services must be accessed by a single login flow (using preferably OAuth2), have access to a single database containing user information and keep the user logged in by keeping the session. HackyDucky in particular also needs for the backend to request additional actions via HTTP.

## Decision

Currently the requirements for services are as follows:
- websocket support
- use of OAuth2
- backend-to-backend and frontend-to-backend communication
- database access and storage
- handling of sessions

Nice to have:
- http2
- SSO
- ease of implementation
- having knowledge of language/framework

And the frameworks/technologies as follows:
- FastAPI
- Express/Bun
- Express/node.js
- pure node.js

These frameworks were chosen based on the author's knowledge of frameworks possibly fulfilling most criteria and familiarity with the modern frameworks in question. This is *not* a comprehensive list of possibilities.

### Framework rundown

#### FastAPI
- websocket support - included in [documentation](https://fastapi.tiangolo.com/advanced/websockets/)
- use of OAuth2 - included in [documentation](https://fastapi.tiangolo.com/tutorial/security/simple-oauth2/)
- backend-to-backend and frontend-to-frontend communication - needs additional library to send requests [(link)](https://stackoverflow.com/questions/63872924/how-can-i-send-an-http-request-from-my-fastapi-app-to-another-site-api)
- database access and storage - needs additional library, integration included in [documentation](https://fastapi.tiangolo.com/tutorial/sql-databases/)
- handling of sessions - possible to add support with additional [plugin](https://jordanisaacs.github.io/fastapi-sessions/guide/getting_started/)
- http2 - needs to be run with [Hypercorn](https://pgjones.gitlab.io/hypercorn/) to be compatible with HTTP2 [(link)](https://fastapi.tiangolo.com/deployment/manually/)
- SSO - possible to add generic provider with additional [plugin](https://tomasvotava.github.io/fastapi-sso/reference/sso.generic/#)
- ease of implementation - subjective, looks easy to read
- having knowledge of language/framework - medium Python knowledge, no framework-specific experience

#### Express/Bun
- websocket support - included in [documentation](https://bun.sh/docs/api/websockets)
- use of OAuth2 - needs additional library - [oauth4webapi](https://github.com/panva/oauth4webapi)
- backend-to-backend and frontend-to-frontend communication - included in [documentation](https://bun.sh/guides/http/fetch)
- database access and storage - included in [documentation](https://bun.sh/docs/api/sqlite)
- handling of sessions - possible to add support with additional [plugin](https://www.npmjs.com/package/express-session)
- http2 - client is supported, but not the server [(link)](https://nodejs.org/api/http2.html)
- SSO - needs additional library [example: passport.js](https://www.passportjs.org/)
- ease of implementation - subjective, clean and easy to understand
- having knowledge of language/framework - high JavaScript knowledge, no Bun experience, some Express experience

#### Express/Node.js
- websocket support - needs additional library - [ws](https://github.com/websockets/ws)
- use of OAuth2 - needs additional library - [example: oauth4webapi](https://github.com/panva/oauth4webapi)
- backend-to-backend and frontend-to-frontend communication - needs additional library [(see list)](https://github.com/request/request/issues/3143)
- database access and storage - needs additional library - [example: mysql](https://www.w3schools.com/nodejs/nodejs_mysql_create_db.asp)
- handling of sessions - possible to add support with additional [plugin](https://www.npmjs.com/package/express-session)
- http2 - included in [documentation](https://nodejs.org/api/http2.html)
- SSO - needs additional library [example: passport.js](https://www.passportjs.org/)
- ease of implementation - subjective, some steps may be tedious to do
- having knowledge of language/framework - high JavaScript knowledge, high Node experience, some Express experience

#### Node.js
- websocket support - needs additional library - [ws](https://github.com/websockets/ws)
- use of OAuth2 - needs additional library - [example: oauth4webapi](https://github.com/panva/oauth4webapi)
- backend-to-backend and frontend-to-frontend communication - needs additional library [(see list)](https://github.com/request/request/issues/3143)
- database access and storage - needs additional library - [example: mysql](https://www.w3schools.com/nodejs/nodejs_mysql_create_db.asp)
- handling of sessions - possible, but may be a hassle [(link)](https://stackoverflow.com/questions/24800495/create-session-in-pure-node-js)
- http2 - included in [documentation](https://nodejs.org/api/http2.html)
- SSO - needs additional library [example: passport.js](https://www.passportjs.org/)
- ease of implementation - subjective, some steps may be tedious to do
- having knowledge of language/framework - high JavaScript knowledge, high Node experience

### Overview

FastAPI comes with the most amount of features already built in or well documented, seemingly only requiring more work with http2 support. But it comes at the cost of possibly longer implementation time, as the author's knowledge of the language and framework is lacking in comparison to JavaScript.

If considering JavaScript related technologies, Bun with Express.js seems the most promising. The only thing missing is http2 support, which (as Bun is still early in development) hopefully will be implemented.

Because of those conditions, it was decided to use FastAPI, being aware of possible setbacks caused by inexperience.

## Consequences

The main consequence of the decision is possibly longer implementation time, and possibly code of worse quality - both because of the developer's inexperience. In return, a lot of setup will already be provided OOTB, and the developer's expertise with the framework will greatly improve.