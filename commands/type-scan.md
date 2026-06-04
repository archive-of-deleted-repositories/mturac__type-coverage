# /type-scan

Scan a specific path for TypeScript type safety issues.

## Usage

```
/type-scan [path]
```

- `path` — optional directory or file path relative to project root (default: `.`)

## Examples

```
/type-scan
/type-scan src/api
/type-scan src/components/DataTable.tsx
```

## What it does

Executes the `type-scan` skill scoped to `[path]`:

1. Finds all `.ts` / `.tsx` files in the given path.
2. Detects unsafe patterns and ranks by severity.
3. Prints a filtered issue table and coverage score for the scoped path.
4. Appends a snapshot to `.type-coverage-history.json`.

Useful for validating a single module before committing or reviewing a PR.
