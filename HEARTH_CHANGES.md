# Hearth integration branch

This branch keeps the Hermes backend features required by the Hearth Android
client on top of the current upstream Hermes `main` branch.

The integration is deliberately opt-in. Ordinary Hermes installations do not
expose the profile-context or project-synchronization resources unless their
corresponding API-server capabilities are enabled in `config.yaml`.

## Commit guide

| Commit | Change | What it does |
| --- | --- | --- |
| `3c40caa708` | Propagate API session identity | Uses the authenticated API session key as the stable user identity for API-created conversations, preserving ownership and attribution. |
| `a1f6e05bfe` | Add profile context endpoint | Provides authenticated, profile-scoped personality and memory context for Hearth. The feature is opt-in, capability-advertised, path-safe, documented, and tested. |
| `e068593072` | Gate project synchronization | Adds the `project_sync` capability switch. Project synchronization remains hidden unless explicitly enabled. |
| `76e8147d4b` | Complete conversation-to-project lifecycle | Persists canonical project assignments, migrates existing assignment data, and removes assignments whenever conversations are permanently deleted. |
| `2eed6c6863` | Expose canonical project-sync API | Adds authenticated project listing, creation, update, archive, deletion, assignment, and unassignment resources backed by Hermes `projects.db`, revision checks, and idempotent creation. |
| `72a2af1b9f` | Harden project synchronization | Makes writes transactional and conflict-safe, validates portable fields, handles archived or deleted replays correctly, and prevents stale assignments across deletion paths. |
| `d2279f5c7c` | Reject ambiguous null descriptions | Rejects `"description": null` instead of returning success without changing data. An empty string remains the explicit way to clear a description. |

## Branch policy

- `main` follows Nous Research upstream.
- `hearth` carries the Android-oriented compatibility layer.
- Hearth-specific changes are pushed only to Rick's fork.
- The capability gates remain disabled by default.
- Upstream updates should be integrated into `main` first, followed by a
  rebase of `hearth` and the project/profile API test suite.

## Verification baseline

The integrated branch passed the focused backend gate covering the API server,
canonical projects database, and session-state lifecycle: **633 tests passed**.
The rewritten commit messages do not alter the source tree.
