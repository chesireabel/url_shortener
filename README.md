
# 🔗 URL Shortener

A fast and simple URL shortening service built with Go, PostgreSQL, and Redis.

## Features

- Shorten long URLs into compact links
- Custom short codes
- Click analytics tracking
- Redis caching for fast redirects
- REST API

## Prerequisites

- Go 1.21+
- PostgreSQL 15+
- Redis 7+

## Quick Start

### 1. Clone and Install

```bash
git clone https://github.com/yourusername/url-shortener.git
cd url-shortener
go mod download
```

### 2. Setup Database

```bash
# Create database
createdb urlshortener

# Run migrations
migrate -path migrations -database "postgresql://postgres:postgres@localhost:5432/urlshortener?sslmode=disable" up
```

### 3. Configure Environment

```bash
cp .env.example .env
# Edit .env with your database credentials
```

### 4. Run

```bash
go run cmd/api/main.go
```

Server runs on `http://localhost:8080`

## API Usage

### Shorten a URL

```bash
curl -X POST http://localhost:8080/api/v1/shorten \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com/very-long-url"}'
```

Response:
```json
{
  "success": true,
  "data": {
    "short_code": "abc123",
    "short_url": "http://localhost:8080/abc123",
    "original_url": "https://example.com/very-long-url"
  }
}
```

### Use Short URL

Visit `http://localhost:8080/abc123` to redirect to original URL.

### Get Analytics

```bash
curl http://localhost:8080/api/v1/analytics/abc123
```

## Environment Variables

```env
# Server
SERVER_PORT=8080

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=urlshortener

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
```

## Project Structure

```
url-shortener/
├── cmd/api/              # Application entry point
├── internal/
│   ├── config/           # Configuration
│   ├── domain/           # Data models
│   ├── repository/       # Database layer
│   ├── service/          # Business logic
│   └── handler/          # HTTP handlers
├── pkg/database/         # DB connections
├── migrations/           # SQL migrations
└── .env                  # Environment config
```

## Development

```bash
# Run tests
go test ./...

# Format code
go fmt ./...

# Build binary
go build -o bin/url-shortener cmd/api/main.go
```

## Docker

```bash
# Build
docker build -t url-shortener .

# Run
docker-compose up -d
```

## License

MIT
