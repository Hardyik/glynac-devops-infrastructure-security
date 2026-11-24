
# DEV-587 – Redis Security Hardening

## 1. Project Overview

Task: DEV-587 – Redis Security Hardening
Team: Nyoman's Squad
Department: Glynac
Technology: Redis 7.4.12, Docker
Environment: Windows with Docker Desktop

## 2. Objective

Identify Redis security weaknesses and implement
security controls to protect authentication,
network access, commands and resources.

## 3. Initial Security Audit

| Security Area | Initial Finding |
|---|---|
| Authentication | No password configured |
| Protected Mode | Disabled |
| Network Binding | Wildcard interfaces |
| Default ACL | Unrestricted access |
| Dangerous Commands | Not restricted |

## 4. Security Improvements Implemented

### Authentication
- Configured password-based authentication.
- Disabled the default Redis user.
- Created a dedicated application user.

### Access Control
- Restricted application access to app:* keys.
- Allowed only required Redis commands.
- Blocked administrative commands for appuser.

### Network Security
- Enabled Redis protected mode.
- Restricted published Docker port to localhost.
- Used an isolated Docker bridge network.

### Command Security
- Disabled FLUSHALL.
- Disabled FLUSHDB.
- Restricted CONFIG and SHUTDOWN through ACL.

### Resource Protection
- Maximum clients: 100
- Maximum memory: 128 MB
- Memory policy: noeviction
- Idle connection timeout: 300 seconds

## 5. Security Validation

| Test | Expected Result | Outcome |
|---|---|---|
| Unauthenticated PING | NOAUTH | PASS |
| Authenticated PING | PONG | PASS |
| SET app:test | OK | PASS |
| GET app:test | Value returned | PASS |
| SET admin:test | NOPERM | PASS |
| FLUSHALL | NOPERM | PASS |
| FLUSHDB | Command unavailable | PASS |
| CONFIG | NOPERM | PASS |
| SHUTDOWN | NOPERM | PASS |
| Docker port mapping | Localhost only | PASS |

## 6. Conclusion

Redis security was improved through authentication,
ACL-based access control, network restrictions,
command restrictions and resource limits.

All listed application-level security tests
produced the expected results.

## 7. Limitations and Future Improvements

- Configure TLS for deployments requiring encrypted
  network communication.
- Integrate Redis monitoring and alerting.
- Review backup and recovery procedures.
- Conduct vulnerability scanning.
- Review resource limits against production workloads.

Note: This implementation was tested in a local
Docker environment and has not been deployed
to Glynac production infrastructure.
