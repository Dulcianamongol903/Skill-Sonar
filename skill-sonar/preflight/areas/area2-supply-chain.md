# Area 2 — Supply Chain & Source Verification (Deep Analysis)

_Check for: unverifiable sources, unknown authors, loose version locks, runtime dependency loading, bootstrap scripts._

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | Known author with history; all dependencies from established registries with exact version locks; source is verifiable |
| Minor | Author is less known but skill is transparent; one or two slightly loose version pins from known registries |
| Moderate | Unknown author with no track record; multiple dependencies without version locks; or dependencies from less-known sources |
| Serious | No verifiable source; downloads from unfamiliar URLs; auto-installing bootstrap tools; dependencies that refresh themselves; or encoded/obfuscated content in dependency references |
| Critical | Prebuilt binaries with no verification; code that loads dependencies at runtime from changeable sources; dependency references that could be silently swapped; or supply chain patterns that prevent reproducible builds |

## Detailed Checks

### Source Provenance
- Where did this skill come from? Is the origin verifiable (e.g., a known repository, a recognized author)?
- Can you trace the skill back to its original source?

### Author Reputation
- Is the author known? Do they have a history of maintaining other skills/packages?
- When was the skill last updated?
- Is there a way to contact the author?

### Community Trust Signals
- Download counts, stars, reviews from other users or agents
- Age of the project
- Active maintenance vs. abandoned

### Dependency Integrity
- Are all packages, imports, and downloads from known registries with exact version locks?
- Are there any dependencies from personal/unknown repositories?
- Could any dependency be subject to a typosquatting attack?

### Encoded Content in Dependencies
- Are dependency references obfuscated, encoded, or dynamically constructed in ways that prevent verification?
- Are download URLs assembled at runtime from variables?

### Bootstrap and Auto-Install Patterns
- Does the skill auto-install tools, download helper scripts, or run setup commands that pull in unverified external code?
- Does it use `curl | sh`, `wget | bash`, or similar pipe-to-shell patterns?
- Does it add package repositories or modify package manager configuration?
