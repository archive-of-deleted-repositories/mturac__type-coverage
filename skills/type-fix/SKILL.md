# type-fix

Interactively suggest and apply type improvements for a specific file.

## Steps

### 1. Load issues for the target file

If a `[file]` argument was provided, scope to that file. Otherwise ask the user to specify one.

Run `type-scan` scoped to the file to produce the issue list.

### 2. Process each issue

For each issue, present a fix proposal in this format:

```
Issue #N — <issue_type> [<SEVERITY>]
File: src/api/client.ts:42

BEFORE:
  function fetchUser(id: any) {

AFTER (suggested):
  function fetchUser(id: string) {

Reasoning: `id` is passed directly to `getUserById(id: string)` on line 47,
so the correct type is `string`.

Apply this fix? [y/n/skip/quit]
```

### 3. Infer better types

For each `any` or untyped location:

- Scan **how the value is used**: what methods are called on it, what it is assigned to, what it is passed into.
- Check surrounding code for compatible interfaces/types already declared in the project.
- Prefer **existing project types** over generic built-ins (`Record<string, unknown>` is a last resort, not a first choice).
- If the correct type cannot be determined, suggest `unknown` instead of `any` (safer).

For `@ts-ignore`:
- Read the next line and explain what TypeScript error it was suppressing.
- Suggest the proper fix (type guard, assertion function, overload, or cast with comment).

For missing return types:
- Infer from the `return` statements in the function body.
- If the function is async, wrap in `Promise<T>`.

### 4. Apply confirmed fixes

After user approval for each fix, apply the edit using the Edit tool.

Show a running tally:

```
Applied: 3 fixes
Skipped: 1
Remaining: 4
```

### 5. Final diff summary

After all fixes are processed:

```
Changes applied to src/api/client.ts:
  - Line 42: any → string
  - Line 67: missing return type → Promise<User>
  - Line 89: @ts-ignore removed, added type guard

Run `type-scan src/api/client.ts` to verify the new score.
```
