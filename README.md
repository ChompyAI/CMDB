# Chompy Catalog (CMDB)

Curated media catalog for Chompy. Contains SQL export files that Chompy instances pull to stay current.

## Structure

```
catalog/
  catalog.sql       <- Full catalog (INSERT OR REPLACE, idempotent)
manifest.json       <- Version metadata for update checking
```

## How Updates Work

1. **cmdb-admin** (`~/dev/cmdb-admin`) manages the catalog database — imports from TMDB, generates embeddings, curates content
2. Running `rake cmdb:publish` exports the catalog and commits it here on a `staging` branch
3. After testing locally against a Chompy instance, merge `staging` → `main` to make it live
4. Chompy instances poll `main` for updates (every 6 hours or manual trigger)

## For Chompy

Raw URL for update checking:
```
https://raw.githubusercontent.com/ChompyAI/CMDB/main/manifest.json
https://raw.githubusercontent.com/ChompyAI/CMDB/main/catalog/catalog.sql
```

## Branching

- `main` — Live catalog. Chompy instances pull from here.
- `staging` — Pending updates. Test locally before merging to main.
