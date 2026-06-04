# type-report

Generate a type safety trend report using historical scan data.

## Steps

### 1. Load history

Read `.type-coverage-history.json` from the project root.

If the file does not exist, print:
```
No history found. Run /type-coverage first to generate a baseline.
```
and stop.

### 2. Overall coverage

Print the current and previous coverage scores:

```
Type Coverage Report — 2026-06-04
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Current:  84.2%  ▲ +3.1% since last run (2026-06-03)
Baseline: 71.0%  (first recorded: 2026-05-01)
```

If only one entry exists, skip the trend line.

### 3. Trend chart

Print a simple ASCII sparkline of coverage over the last 10 runs:

```
Coverage trend (last 10 runs):
  71 ▁▂▃▃▄▄▅▆▇█ 84
  Date range: 2026-05-01 → 2026-06-04
```

State whether the codebase is **improving**, **degrading**, or **stable** (< 1% change over last 3 runs).

### 4. Breakdown by directory

Run a fresh scan (or use the latest snapshot if run within the last hour) and group issues by top-level directory:

```
Worst-typed directories:
  src/api/          — 34 issues  (coverage: 62%)
  src/components/   — 21 issues  (coverage: 71%)
  src/utils/        — 8 issues   (coverage: 88%)
  src/hooks/        — 5 issues   (coverage: 94%)
```

### 5. Top type-debt contributors

List the top 10 files with the most HIGH-severity issues:

```
Top type-debt files:
  1. src/api/client.ts          — 14 HIGH issues
  2. src/components/DataTable.tsx — 9 HIGH issues
  3. src/utils/transform.ts       — 6 HIGH issues
  ...
```

### 6. Recommendations

Based on the data, output up to 3 actionable recommendations:

- If `any` count is > 20: "Enable `noImplicitAny` in tsconfig.json to catch new regressions automatically."
- If `ts-ignore` count is > 5: "Audit @ts-ignore comments — run `/type-fix` on the top offenders."
- If coverage dropped > 5% since last run: "Coverage regressed. Check recent commits for newly introduced `any` types."
- If coverage > 95%: "Excellent coverage. Consider enabling `strict: true` in tsconfig if not already set."

### 7. Save updated history

The `type-scan` skill already appends to `.type-coverage-history.json`; this skill only reads it. Do not write duplicate entries.
