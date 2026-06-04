# type-scan

Scan the codebase for TypeScript type safety issues and produce a severity-ranked report.

## Steps

### 1. Discover files

Find all `.ts` and `.tsx` files, excluding `.d.ts`, `node_modules`, and `dist`:

```bash
find . -type f \( -name "*.ts" -o -name "*.tsx" \) \
  ! -name "*.d.ts" \
  ! -path "*/node_modules/*" \
  ! -path "*/dist/*" \
  ! -path "*/.next/*" \
  ! -path "*/build/*"
```

If a `[path]` argument was provided to the command, scope the search to that directory instead of `.`.

### 2. Scan for unsafe patterns

For each file, detect the following patterns and record `file`, `line`, `column`, `issue_type`, and `severity`:

| Pattern | Issue Type | Severity |
|---|---|---|
| `: any` in variable/param/return position | `explicit-any` | HIGH |
| `as any` | `cast-to-any` | HIGH |
| `<any>` type assertion | `generic-any` | HIGH |
| `@ts-ignore` comment | `ts-ignore` | HIGH |
| `@ts-expect-error` comment | `ts-expect-error` | HIGH |
| Exported function with no return type annotation | `missing-return-type` | MEDIUM |
| Function parameter with no type annotation | `untyped-param` | MEDIUM |
| `as SomeType` where source type is `unknown` or `{}` | `unsafe-cast` | MEDIUM |
| Object literal assigned with `as` instead of typed variable | `object-as-cast` | LOW |

Use grep patterns for fast scanning:

```bash
# HIGH: explicit any
grep -rn ": any[^A-Za-z]" --include="*.ts" --include="*.tsx" .

# HIGH: cast to any
grep -rn "\bas any\b\|<any>" --include="*.ts" --include="*.tsx" .

# HIGH: ts-ignore / ts-expect-error
grep -rn "@ts-ignore\|@ts-expect-error" --include="*.ts" --include="*.tsx" .

# MEDIUM: exported functions missing return type (heuristic)
grep -rn "^export\s\+\(async\s\+\)\?function\s\+[A-Za-z]\+" --include="*.ts" --include="*.tsx" .

# MEDIUM: untyped params (heuristic — params with no colon before comma/paren)
grep -rn "([a-zA-Z_$][a-zA-Z0-9_$]*[^:,)=\s][^:]*)" --include="*.ts" --include="*.tsx" .
```

### 3. Calculate type coverage score

Approximate coverage score:

```
typed_identifiers   = count of params/variables with explicit type annotations
total_identifiers   = typed_identifiers + untyped_identifiers (from untyped-param + explicit-any counts)
coverage_score      = (typed_identifiers / total_identifiers) * 100
```

Report as: `Type Coverage: 84.2% (typed: 421 / total: 500)`

### 4. Output

Print a markdown table sorted by severity (HIGH → MEDIUM → LOW):

```
| File | Line | Issue Type | Severity | Snippet |
|------|------|-----------|----------|---------|
| src/api/client.ts | 42 | explicit-any | HIGH | `data: any` |
| src/hooks/useUser.ts | 17 | ts-ignore | HIGH | `// @ts-ignore` |
| src/utils/format.ts | 8 | missing-return-type | MEDIUM | `export function fmt(` |
```

Then print:

```
Top 10 files by issue count:
1. src/api/client.ts — 14 issues
2. src/components/Table.tsx — 9 issues
...

Summary:
  HIGH:   23 issues
  MEDIUM: 41 issues
  LOW:     7 issues
  Total:  71 issues
  Coverage: 78.3%
```

### 5. Save snapshot

Append a snapshot entry to `.type-coverage-history.json` in the project root (create if absent):

```json
[
  {
    "date": "2026-06-04T00:00:00Z",
    "coverage": 78.3,
    "high": 23,
    "medium": 41,
    "low": 7,
    "total": 71
  }
]
```
