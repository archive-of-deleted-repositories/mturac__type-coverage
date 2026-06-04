# /type-coverage

Run a full TypeScript type safety scan across the entire codebase.

## Usage

```
/type-coverage
```

## What it does

Executes the `type-scan` skill on the project root:

1. Discovers all `.ts` / `.tsx` files (excluding `node_modules`, `dist`, `.d.ts`).
2. Detects all unsafe patterns: `any`, `as any`, `@ts-ignore`, missing return types, untyped parameters, unsafe casts.
3. Outputs a severity-ranked issue table (HIGH → MEDIUM → LOW).
4. Calculates and prints the overall type coverage score.
5. Lists the top 10 files with the most type issues.
6. Appends a snapshot to `.type-coverage-history.json` for trend tracking.

## Output example

```
Type Coverage Scan — src/
━━━━━━━━━━━━━━━━━━━━━━━━━

| File                        | Line | Issue Type         | Severity | Snippet              |
|-----------------------------|------|--------------------|----------|----------------------|
| src/api/client.ts           |   42 | explicit-any       | HIGH     | data: any            |
| src/api/client.ts           |   67 | ts-ignore          | HIGH     | // @ts-ignore        |
| src/hooks/useUser.ts        |   17 | missing-return-type| MEDIUM   | export function use( |
| src/utils/format.ts         |    8 | untyped-param      | MEDIUM   | function fmt(value)  |

Summary:
  HIGH:   23 issues
  MEDIUM: 41 issues
  LOW:     7 issues
  Total:  71 issues

Type Coverage: 78.3%
Snapshot saved to .type-coverage-history.json
```

## See also

- `/type-scan [path]` — scan a specific path
- `/type-fix [file]` — interactively fix issues in a file
- `/type-report` — show coverage trend and directory breakdown
