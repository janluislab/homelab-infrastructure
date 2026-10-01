# Incident — Immich vector-extension and PostgreSQL image compatibility

[All incidents](README.md) · [Migration preparation](../migration-preparation.md)

## Impact and evidence

During the original Windows/Docker preparation, Immich reported:

```text
No vector extension found. Available extensions: vchord, vector.
```

The database image and the application's expected vector-extension support needed to be checked together. The previous image was `tensorchord/pgvecto-rs:pg14-v0.2.0`.

## Recorded change

The database image was changed to:

```text
ghcr.io/immich-app/postgres:14-vectorchord0.4.3-pgvectors0.2.0
```

The August 14 output showed PostgreSQL healthy, existing database data retained rather than initialized as a new database, and Immich performing VectorChord initialization/reindexing. Logs included index work for existing face and CLIP data.

## Result and diagnostic boundary

The extension/image compatibility blocker was addressed. A subsequent geodata/metadata import reported `CONNECTION_CLOSED` / `ECONNRESET`, which was a separate failure observation. Completion of vector initialization did not prove that every later application startup step had succeeded.

By October 1, the recorded PostgreSQL container still used the VectorChord-compatible image, and server/database/machine-learning health plus user-visible application functionality were confirmed.

## Lesson applied

Check an application's supported database image and extensions together, especially when moving between application versions or hosts. Preserve existing data, retain a logical backup, and validate both the database and the application after the change.

The pinned historical PostgreSQL image is evidence of the deployed compatibility result, not an instruction to change database versions blindly. See the [current baseline](../current-state.md), [backup record](../../backup/restic-strategy.md), and [PostgreSQL filesystem incident](postgresql-corruption.md) for the separate storage-layer issue.
