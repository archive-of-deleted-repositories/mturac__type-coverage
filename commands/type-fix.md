# /type-fix

Interactively fix type issues in a specific file.

## Usage

```
/type-fix [file]
```

- `file` — path to the TypeScript file to fix (required)

## Examples

```
/type-fix src/api/client.ts
/type-fix src/components/DataTable.tsx
```

## What it does

Executes the `type-fix` skill on the given file:

1. Scans the file for all type issues.
2. For each issue, presents a before/after diff with a specific type suggestion.
3. Explains the reasoning (how the variable is used, what existing types match).
4. Asks for confirmation before applying each change.
5. Shows a running tally of applied/skipped fixes.
6. Prints a final diff summary.

## Interactive prompts

```
Issue #1 — explicit-any [HIGH]
File: src/api/client.ts:42

BEFORE:  function fetchUser(id: any) {
AFTER:   function fetchUser(id: string) {

Reasoning: id is passed to getUserById(id: string) on line 47.

Apply? [y/n/skip/quit]
```
