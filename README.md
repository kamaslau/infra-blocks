# infra-blocks

Minimal, disposable service units for standing up a working environment on a machine you have never seen before.

Each unit is one repository. Clone it, copy `.env.example` to `.env`, run `docker compose up -d`, and you have a service listening on a documented port with a documented credential. `docker compose down -v` removes all trace of it.

Nothing here is clever. Everything here is meant to survive being run by someone who did not write it — on a client's own hardware, behind their firewall, without a vendor console in the loop.

## Why this exists

Most integration work in China fails at the same three places: the image registry cannot be reached, the certificate cannot be issued because port 80 is closed, and nobody can say afterwards what was running where.

These units are built around those three failures. Each one documents its registry mirror configuration, its TLS strategy, and the exact ports and volumes it occupies.

## Units

Two groups. **Runtime** serves traffic or keeps the machines talking; **storage** still holds data after the process stops.

Every unit follows the same rules: one service, one port, one documented credential, removed with `docker compose down -v`. The ports below are defaults — each one is a variable in `.env` (`.env.sample` in a few units) and can be changed without touching the compose file.

### Runtime

| Unit | What it gives you | Default port | When a client needs it |
|---|---|---|---|
| [caddy](https://github.com/kamaslau/infra-blocks-caddy) | Automatic TLS reverse proxy, DNS-01 via Cloudflare | 80, 443 (+ 443/udp) | Port 80 is closed by policy, or the domain sits behind Cloudflare |
| [gitea](https://github.com/kamaslau/infra-blocks-gitea) | Self-hosted Git | 3000 (web), 2222 (SSH) | Code cannot leave the intranet, or a contractor handover needs a private repo |
| [k3s](https://github.com/kamaslau/infra-blocks-k3s) | Single-binary Kubernetes, with Tailscale joining cloud and on-prem nodes into one network | 6443 (API server) | They have a server room *and* cloud hosts, and want one place to deploy |
| [prometheus](https://github.com/kamaslau/infra-blocks-prometheus) | Prometheus + Grafana + node-exporter | 9090 · 3000 (Grafana) · 9100 | The first time they ask "how do we know the server is still up" |
| [verdaccio](https://github.com/kamaslau/infra-blocks-verdaccio) | Private npm registry | 4873 | Private packages cannot go public, and builds must be reproducible offline |
| [wordpress](https://github.com/kamaslau/infra-blocks-wordpress) | WordPress on MySQL | 80 | They want a site they can edit themselves, not a developer for every sentence |

### Storage

| Unit | What it gives you | Default port | When a client needs it |
|---|---|---|---|
| [influxdb](https://github.com/kamaslau/infra-blocks-influxdb) | InfluxDB 3 with its own query UI | 8181 (HTTP), 8888 (UI) | Time series from sensors — the IoT side of the house |
| [mongodb](https://github.com/kamaslau/infra-blocks-mongodb) | MongoDB with the official Node.js driver | 27017 | Document data, or a schema that is still moving |
| [mysql](https://github.com/kamaslau/infra-blocks-mysql) | MySQL / MariaDB with Adminer | 3306 (Adminer 8080) | Migrating off a legacy system that was already MySQL |
| [neo4j](https://github.com/kamaslau/infra-blocks-neo4j) | Neo4j with the official Node.js driver | 7687 (Bolt), 7474 (browser) | The question is about multi-hop relationships a join table cannot express |
| [postgresql](https://github.com/kamaslau/infra-blocks-postgresql) | PostgreSQL with Adminer | 5432 (Adminer 8081) | Someone other than you needs to open the data and look at it |
| [redis](https://github.com/kamaslau/infra-blocks-redis) | Redis with the official Node.js client | 6379 | Cache, session store, or a job queue |

Prometheus sits with runtime rather than storage: it does keep samples, but what you operate is a monitoring service. The two database units that ship Adminer list its port next to the database's, because that is the port the person you are handing over to will actually open.

## Not in this list

Application scaffolds — [fiber](https://github.com/kamaslau/trial-fiber), [actix-web](https://github.com/kamaslau/trial-actix-web), [flask](https://github.com/kamaslau/trial-flask), [koajs](https://github.com/kamaslau/trial-koajs), [electron](https://github.com/kamaslau/trial-electron), [tauri](https://github.com/kamaslau/trial-tauri) — are kept separate. They answer "what do I write the app in", which is a different question from "how do I make it run there".

## Conventions

- Ports are declared in `.env`, not hardcoded.
- Credentials in the README are for local use only and are marked as such.
- Every unit has a `Verify` section with a command that proves the service actually answers.
- Ports are published on the host, not bound to `127.0.0.1` by default — these units are meant for a machine you already control. On a host with a public interface, change `${PORT}:…` to `127.0.0.1:${PORT}:…` before the first `up`.

## License

MIT — see [LICENSE](https://github.com/kamaslau/infra-blocks/blob/main/LICENSE). Take what is useful.

## Maintained by

Kamas Lau — [kamaslau.com](https://www.kamaslau.com/works?utm_source=github&utm_medium=profile&utm_campaign=always-on) · I do this for foreign companies operating in China, on their own infrastructure.