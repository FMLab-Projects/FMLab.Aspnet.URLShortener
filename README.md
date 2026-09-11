# FMLab.Aspnet.URLShortener

URL shortener built with ASP.NET Core Minimal APIs.

## Features

- Create, update, delete and redirect short links, with click analytics per link
- Optional custom aliases, with availability check endpoint
- Choice of temporary (307) or permanent (301) redirection
- API key authentication, rate limiting and versioned API (`/api/v1`)
- Redis caching for redirection lookups, PostgreSQL for persistence (EF Core)
- Health checks and Swagger/OpenAPI documentation

## Tech Stack

- .NET 8, ASP.NET Core Minimal APIs
- Entity Framework Core + PostgreSQL (Npgsql)
- StackExchange.Redis
- Docker / Docker Compose (+ nginx reverse proxy for production/homelab)

## Project Structure

- `FMLab.Aspnet.URLShortener.Api` — HTTP endpoints, authentication, versioning and app configuration
- `FMLab.Aspnet.URLShortener.Business` — domain entities, services and DTOs
- `FMLab.Aspnet.URLShortener.Infrastructure` — EF Core persistence, Redis cache, repositories
- `docker/` — Dockerfile and Compose files for local, production and homelab deploys
- `infra/` — database and nginx configuration

## Getting Started

```bash
cp .env.example .env
# fill in API_KEY, DB_* and REDIS_* values
docker compose -f docker/docker-compose.yml up --build
```

The API will be available at `http://localhost:9000`, with Swagger at `/swagger`.

Sample requests are available in `src/FMLab.Aspnet.URLShortener.Api/FMLab.Aspnet.URLShortener.Api.http`.

## Live Demo

<a href="https://shortener.fmlab.com.br">https://shortener.fmlab.com.br</a>

## License

MIT — see [LICENSE](LICENSE).
