# /type-report

Show the type coverage trend and directory breakdown for the project.

## Usage

```
/type-report
```

## What it does

Executes the `type-report` skill:

1. Reads `.type-coverage-history.json` to load historical scan data.
2. Prints current vs previous coverage with delta.
3. Renders an ASCII sparkline for the last 10 runs.
4. States whether coverage is improving, degrading, or stable.
5. Breaks down issues by top-level directory.
6. Lists the top 10 files with the most HIGH-severity issues.
7. Outputs up to 3 actionable recommendations.

## Example output

```
Type Coverage Report — 2026-06-04
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Current:  84.2%  ▲ +3.1% since last run
Baseline: 71.0%  (first recorded: 2026-05-01)

Coverage trend: ▁▂▃▄▄▅▆▇█  (improving)

Worst-typed directories:
  src/api/         — 34 issues  (62%)
  src/components/  — 21 issues  (71%)

Top type-debt files:
  1. src/api/client.ts — 14 HIGH issues

Recommendations:
  - Enable noImplicitAny in tsconfig.json
  - Run /type-fix on top offenders
```

## Prerequisite

Run `/type-coverage` at least once to generate `.type-coverage-history.json`.
