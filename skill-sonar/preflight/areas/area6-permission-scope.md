# Area 6 — Permission & Access Scope (Deep Analysis)

_Check for: disproportionate permissions, privilege escalation, system-level access beyond stated purpose. Core test: "Is this access necessary for what the skill claims to do?"_

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | All file, network, and command access is clearly necessary for the stated purpose; scope is minimal |
| Minor | Slightly broader access than strictly necessary, but clearly documented and justifiable |
| Moderate | Accesses files or directories beyond its stated scope without clear justification; or runs commands that could affect areas outside the skill's purpose |
| Serious | Requires system-level access disproportionate to its function; modifies system configuration files; or requests network access when the stated purpose is purely local |
| Critical | Requests root/admin privileges; modifies system-wide configuration (PATH, shell profiles, OS settings); installs system services; or access scope is so broad it could do essentially anything |

## Detailed Checks

### File Access Scope
- Which files and directories does the skill read? Write? Delete?
- Is this set minimal for the skill's purpose?
- Does it access files outside the project/workspace?
- Does it use broad globs that could match unintended files?

### Command Execution Scope
- What shell commands does the skill run?
- Are commands scoped to the project, or do they affect the broader system?
- Does it execute dynamically constructed commands (higher risk)?
- Are there any pipes to shell interpreters (`sh`, `bash`, `cmd`, `powershell`)?

### Network Access Scope
- Does the skill require network access? To which endpoints?
- Is network access necessary for its stated purpose?
- Are endpoints hardcoded, configurable, or dynamically determined?
- Does it open listening ports?

### System Modification
- Does the skill modify system-level files (`/etc/`, registry, shell profiles)?
- Does it alter environment variables globally?
- Does it install system services, daemons, or scheduled tasks?
- Does it modify other applications' configuration?

### Privilege Escalation
- Does the skill use `sudo`, `runas`, or request elevated permissions?
- Could it escalate privileges through indirect means (e.g., modifying a script that runs with elevated permissions)?
- Does it modify file permissions or ownership?

### Proportionality Test
For every permission the skill requires, ask: **"Is this necessary for what the skill claims to do?"** A mismatch between stated purpose and required permissions is a red flag. Document each permission alongside its justification (or lack thereof).
