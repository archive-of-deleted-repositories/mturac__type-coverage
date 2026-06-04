# type-coverage

Claude Code plugin — track TypeScript type safety across your codebase.

Finds every `any`, unsafe cast, missing return type, and untyped parameter. Scores your coverage. Tracks the trend over time.

## Install

```bash
claude plugin install type-coverage
```

Or clone locally and load from path:

```bash
git clone https://github.com/mturac/type-coverage
claude plugin install ./type-coverage
```

## Commands

| Command | Description |
|---|---|
| `/type-coverage` | Full scan of the entire project |
| `/type-scan [path]` | Scan a specific file or directory |
| `/type-fix [file]` | Interactively fix issues in one file |
| `/type-report` | Coverage trend and directory breakdown |

## Example scan output

```
Type Coverage Scan — src/
━━━━━━━━━━━━━━━━━━━━━━━━━

| File                        | Line | Issue Type          | Severity |
|-----------------------------|------|---------------------|----------|
| src/api/client.ts           |   42 | explicit-any        | HIGH     |
| src/api/client.ts           |   67 | ts-ignore           | HIGH     |
| src/hooks/useUser.ts        |   17 | missing-return-type | MEDIUM   |
| src/utils/format.ts         |    8 | untyped-param       | MEDIUM   |

Summary:
  HIGH:   23
  MEDIUM: 41
  LOW:     7
  Total:  71

Type Coverage: 78.3%
```

## What gets flagged

| Pattern | Severity |
|---|---|
| `: any`, `as any`, `<any>` | HIGH |
| `@ts-ignore`, `@ts-expect-error` | HIGH |
| Exported function with no return type | MEDIUM |
| Function parameter with no type annotation | MEDIUM |
| Unsafe cast from `unknown` or `{}` | MEDIUM |
| Object literal with `as` instead of typed variable | LOW |

## Coverage history

Every scan appends a snapshot to `.type-coverage-history.json`:

```json
[
  { "date": "2026-06-04T00:00:00Z", "coverage": 78.3, "high": 23, "medium": 41, "low": 7, "total": 71 }
]
```

Run `/type-report` to see the trend, directory breakdown, and recommendations.
