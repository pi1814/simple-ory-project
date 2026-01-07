# Simple Ory Project

## Folder Structure

```
simple-ory-project/
  identity-provider/      # Hydra & Kratos config, custom login/consent UI
  client-app/             # OAuth2 client app (Express)
```

## Quick Start

### 1. Prerequisites

- Docker & Docker Compose
- Node.js (v12+)
- Git

### 2. Hydra & Kratos Setup

```sh
docker compose -f identity-provider/quickstart.yml up --build
```

Hydra runs on:
- Public: `http://0.0.0.0:4444`
- Admin: `http://0.0.0.0:4445`

Kratos runs on:
- Public: `http://0.0.0.0:4433`
- Admin: `http://0.0.0.0:4434`

Custom UI runs on:
- `http://0.0.0.0:4455`

### 3. Create OAuth2 Client

```sh
docker exec -it identity-provider-hydra-1 hydra create oauth2-client --endpoint http://0.0.0.0:4445 --redirect-uri http://0.0.0.0:4000/callback  --name "My Node.js App" --grant-type authorization_code,refresh_token --response-type code --scope openid,offline_access,email
```

Copy the `CLIENT_ID` & `CLIENT_SECRET` into `client-app/.env`.

### 4. Client App

```sh
cd client-app
npm install
npm start
```

Runs on: `http://0.0.0.0:4000`

### 5. Test the Flow

1. Go to `http://0.0.0.0:4000`
2. Click "Login with OAuth2"
3. Register/login at the custom UI (`http://0.0.0.0:4455`)
4. Grant consent
5. Redirected back to client app with access token

