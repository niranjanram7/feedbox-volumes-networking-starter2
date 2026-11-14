# FeedBox volumes and networking starter

FeedBox is a minimal feedback collector with PostgreSQL (`db`), Express (`api`), and an Nginx-served frontend (`web`).

## Project structure

```text
├── server.js
├── Dockerfile
├── package.json
├── package-lock.json
├── docker-compose.yml
├── .env.example
└── client/
    ├── Dockerfile
    └── index.html
```

## Run the starter

```bash
cp .env.example .env
docker compose up --build
```

- API: http://localhost:5000/api/messages
- Frontend: http://localhost:3000

The main Compose file keeps PostgreSQL data in the `feedbox_data` named volume.
The `backend` network connects the API to PostgreSQL, while the `frontend`
network connects the browser-facing web service to the API. The API joins both
networks; PostgreSQL is not exposed to the host. The API port is published so
the browser can reach it at `localhost:5000`.

For local development, Compose automatically loads `docker-compose.override.yml`.
It mounts `server.js` into the API container and runs it with Nodemon, so API
changes reload without rebuilding the image. Use `docker compose -f
docker-compose.yml up --build` when you want only the base configuration.

`DATABASE_URL` uses `db` as its hostname because the API resolves the database
through Compose's backend network. `localhost` inside the API container would
refer to the API container itself, not PostgreSQL.

## Before changing anything

1. Open `docker-compose.yml` and trace how each service currently starts.
2. Identify where PostgreSQL writes data inside its container.
3. Check which services genuinely need to communicate with each other.
4. Notice which development behaviour should not belong in the main Compose file.

Use the assignment brief's expected behaviours as your completion checklist. Choose the required Compose configuration yourself.

Validate the final configuration with:

```bash
docker compose config -q
docker compose up --build
```

Do not commit `.env` or `node_modules`.
