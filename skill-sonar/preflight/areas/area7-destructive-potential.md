# Area 7 — Destructive Potential (Deep Analysis)

_Could this skill delete, overwrite, or damage your files?_

## Severity Rubric

| Rating     | What It Looks Like                                                                 |
| ---------- | ---------------------------------------------------------------------------------- |
| No Concern | No destructive actions; all changes are additive or confirmed                      |
| Minor      | Small, scoped deletions with user confirmation                                     |
| Moderate   | Broader destructive operations (glob-based deletes, reset-and-rebuild)             |
| Serious    | Unconditional destructive commands, or "cleanup" language hiding broad deletions   |
| Critical   | Chained destructive commands, force flags, or user input feeding into delete paths |

## Detailed Checks

### Destructive Commands

Scan for every instance of:

- Delete/remove operations: `rm`, `del`, `remove`, `unlink`, `rmdir`, `shutil.rmtree`
- Overwrite operations: `>` redirect (truncate), `tee`, `dd`, force-write
- Reset operations: `git reset --hard`, `git clean -fd`, database `DROP`, `TRUNCATE`
- Wipe/prune operations: `wipe`, `prune`, `purge`, `nuke`

### Safety Mechanisms

For each destructive action found, verify:

- Is there user confirmation before execution?
- Is the scope clearly limited (specific files vs. globs vs. entire directories)?
- Is the action reversible (can it be undone)?
- Are there dry-run or preview options?

### Disguised Destruction

- "Cleanup" or "housekeeping" language hiding broad deletions
- "To ensure freshness, we delete all generated files" — how broad is "all"?
- Reset-and-rebuild patterns where the "reset" part is destructive

### Dangerous Command Chains

- Sequences where one failure cascades into destructive outcomes
- `command1 && rm -rf $DIR` where `$DIR` could be unset (expanding to `/`)
- Pipelines where intermediate failure leads to data loss

### Force Flags

- `--force`, `-f`, `--no-preserve-root`, `--yes`, `-y`
- Any flag that bypasses safety prompts or confirmation dialogs

### User Input in Destructive Paths

- Variables or user input that could accidentally feed into destructive commands
- Unvalidated paths passed to delete operations
- Template strings where injection could alter the delete target
