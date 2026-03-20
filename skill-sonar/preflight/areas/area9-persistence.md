# Area 9 — Persistence (Deep Analysis)

_Check for: persistent state or system changes **beyond** the skill's own install directory. Files in the skill directory are normal — only flag extra persistence._

## Severity Rubric

| Rating     | What It Looks Like                                                                                                                                                      |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| No Concern | No persistent artifacts beyond the normal skill install directory; nothing lingers after uninstall                                                                      |
| Minor      | Small cache files in a clearly scoped directory, documented and easy to clean up                                                                                        |
| Moderate   | Writes to shared locations outside its own directory (git hooks, user settings, shared config) — documented                                                             |
| Serious    | Installs hooks, startup tasks, or PATH changes that aren't clearly documented; or creates state files outside the project                                               |
| Critical   | Hidden persistent changes that auto-restore when you try to remove them; or system-level persistence mechanisms (services, scheduled jobs, shell profile modifications) |

## Detailed Checks

### Project-Level Persistence

- Git hooks (`.git/hooks/`) — pre-commit, post-checkout, etc.
- Editor/IDE settings or extensions installed without consent
- Build system modifications (adding scripts to package.json, Makefile targets)
- Configuration file modifications (`.eslintrc`, `tsconfig.json`, etc.)

### User-Level Persistence

- Shell profile modifications (`~/.bashrc`, `~/.zshrc`, `~/.profile`, PowerShell profile)
- PATH modifications
- Alias or function definitions added to shell config
- User-level cron jobs or scheduled tasks
- Files written to `~/`, `~/.config/`, or other user directories

### System-Level Persistence

- System services or daemons installed
- System-wide cron jobs
- Modifications to `/etc/` or system registry
- Boot scripts or startup items
- Network configuration changes

### Stealth Persistence

- Changes that auto-restore when you try to remove them
- Watchdog processes that reinstall removed components
- Hidden files or directories (dotfiles in unexpected locations)
- Persistence through legitimate-looking mechanisms (e.g., abusing git hooks to reinstall)

### Cleanup Assessment

- Can all changes be cleanly reversed by removing the skill's directory?
- Are there documented uninstall steps?
- Would a user know about all persistent artifacts to clean up?
