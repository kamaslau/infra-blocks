# infra-blocks

Minimal, disposable service units for standing up a working environment on a machine you have never seen before.

Each unit is one repository. Clone it, copy `.env.example` to `.env`, run `docker compose up -d`, and you have a service listening on a documented port with a documented credential. `docker compose down -v` removes all trace of it.

Nothing here is clever. Everything here is meant to survive being run by someone who did not write it — on a client's own hardware, behind their firewall, without a vendor console in the loop.

## Why this exists

Most integration work in China fails at the same three places: the image registry cannot be reached, the certificate cannot be issued because port 80 is closed, and nobody can say afterwards what was running where.

These units are built around those three failures. Each one documents its registry mirror configuration, its TLS strategy, and the exact ports and volumes it occupies.

## Units

| Unit | What it gives you | When a client needs it |
|---|---|---|
| [k3s](https://github.com/kamaslau/infra-blocks-k3s) | Single-binary Kubernetes, with Tailscale joining cloud and on-prem nodes into one network | They have a server room *and* cloud hosts, and want one place to deploy |
| [caddy](https://github.com/kamaslau/infra-blocks-caddy) | Automatic TLS reverse proxy, DNS-01 via Cloudflare | Port 80 is closed by policy, or the domain sits behind Cloudflare |
| [postgresql](https://github.com/kamaslau/infra-blocks-postgresql) | PostgreSQL with Adminer | Someone other than you needs to open the data and look at it |
| [mysql](https://github.com/kamaslau/infra-blocks-mysql) | MySQL / MariaDB with Adminer | Migrating off a legacy system that was already MySQL |
| [mongodb](https://github.com/kamaslau/infra-blocks-mongodb) | MongoDB with the official Node.js driver | Document data, or a schema that is still moving |
| [redis](https://github.com/kamaslau/infra-blocks-redis) | Redis with the official Node.js driver | Cache, session store, or a job queue |
| [neo4j](https://github.com/kamaslau/infra-blocks-neo4j) | Neo4j with the official Node.js driver | The question is about multi-hop relationships a join table cannot express |
| [influxdb](https://github.com/kamaslau/infra-blocks-influxdb) | InfluxDB | Time series from sensors — the IoT side of the house |
| [prometheus](https://github.com/kamaslau/infra-blocks-prometheus) | Prometheus + Grafana + node-exporter | The first time they ask "how do we know the server is still up" |
| [gitea](https://github.com/kamaslau/infra-blocks-gitea) | Self-hosted Git | Code cannot leave the intranet, or a contractor handover needs a private repo |
| [verdaccio](https://github.com/kamaslau/infra-blocks-verdaccio) | Private npm registry | Private packages cannot go public, and builds must be reproducible offline |
| [wordpress](https://github.com/kamaslau/infra-blocks-wordpress) | WordPress | They want a site they can edit themselves, not a developer for every sentence |

## Not in this list

Application scaffolds — [fiber](https://github.com/kamaslau/trial-fiber), [actix-web](https://github.com/kamaslau/trial-actix-web), [flask](https://github.com/kamaslau/trial-flask), [koajs](https://github.com/kamaslau/trial-koajs), [electron](https://github.com/kamaslau/trial-electron), [tauri](https://github.com/kamaslau/trial-tauri) — are kept separate. They answer "what do I write the app in", which is a different question from "how do I make it run there".

## Conventions

- Ports are declared in `.env`, not hardcoded.
- Credentials in the README are for local use only and are marked as such.
- Every unit has a `Verify` section with a command that proves the service actually answers.
- Nothing binds to `0.0.0.0` by default.

## Maintained by

Kamas Lau — [kamaslau.com](https://www.kamaslau.com/works?utm_source=github&utm_medium=profile&utm_campaign=always-on) · I do this for foreign companies operating in China, on their own infrastructure.