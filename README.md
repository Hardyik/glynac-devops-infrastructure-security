
# DEV-587 – Redis Security Hardening

A practical Redis security hardening implementation
using Docker and Redis ACL.

## Technologies
- Redis 7.4.12
- Docker
- Docker Compose
- PowerShell
- Redis ACL

## Features
- Password authentication
- Default user disabled
- Restricted application user
- Key-level access restrictions
- Protected mode enabled
- Localhost-only port publishing
- Dangerous command restrictions
- Memory and connection limits

## Project Structure

```text
.
├── config/
│   ├── redis.conf
│   └── users.acl
├── docs/
│   └── initial-audit.md
├── screenshots/
├── docker-compose.yml
├── .gitignore
└── README.md
```

## Setup

1. Install Docker Desktop.
2. Clone this repository.
3. Create your local config/users.acl file.
4. Configure a Redis ACL user with a strong password.
5. Start Redis:

```powershell
docker compose up -d
```

6. Check the container:

```powershell
docker ps
```

## Security Testing

Test authentication:

```powershell
docker exec redis-dev587 redis-cli PING
```

Expected: NOAUTH.

Test authenticated access:

```powershell
docker exec -it redis-dev587 redis-cli --user appuser --askpass
```

Then:

```redis
PING
SET app:test "Redis secured"
GET app:test
```

## Security Documentation

See `docs/initial-audit.md` for the audit findings,
implemented controls and validation results.

## Important

The ACL file contains credential material and must
never be committed to Git.

This project is a local security demonstration.
Production deployment requires additional review.
