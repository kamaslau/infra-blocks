# Contributing

This repository is the index for [infrastructure blocks](https://github.com/kamaslau/infra-blocks): a set of small, disposable units for standing up services on a machine you have never seen before.

## What belongs in the list

A unit earns a row when all four are true:

1. It runs with one `docker compose up -d`, from a clean clone, with no undocumented step.
2. It has one port and one documented credential.
3. It can be removed with `docker compose down -v`, leaving nothing behind.
4. Someone who did not write it can hand it over to someone else using only its README.

A unit that needs a second machine, a control plane, or a licence server does not belong here yet. Those are projects, not units.

## Opening a unit

- **Runtime** if it serves traffic or keeps machines talking: proxies, orchestrators, registries, CMS, monitoring.
- **Storage** if it holds data after the process stops.

The row carries the unit's default port. If the port is a variable in `.env`, write the default, not the variable name.

## Changes to this repository

- Keep the tables alphabetical inside each group. Order that looks arbitrary invites re-sorting, and re-sorting loses the port column to a merge conflict.
- The description of a unit in the table and the description on GitHub must agree. Two descriptions of one thing is how a reader learns to trust neither.
- The README is English. Chinese belongs in the unit's own *Notes for deployments in China* section.

## Reporting a problem

Open an issue with the unit's name, the command you ran, and the output of `docker compose ps`. If a unit's `Verify` command passes but the service is not usable, say so — that is the most valuable bug report this list can get.

---

Part of [infrastructure blocks](https://github.com/kamaslau/infra-blocks) · MIT
