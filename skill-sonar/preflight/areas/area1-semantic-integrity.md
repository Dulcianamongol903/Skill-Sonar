# Area 1 — Semantic & Structural Integrity (Deep Analysis)

_Check for: internal contradictions, hidden/encoded content, invisible characters, description-behavior mismatch, dishonest structure._

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | Description matches behavior; content is consistent across all sections; no hidden content detected |
| Minor | Description is slightly vague but actions seem reasonable; structure is clean |
| Moderate | Some actions aren't mentioned in the description; or minor internal contradictions between sections (e.g., a "read-only" claim in the intro but write operations in the body) |
| Serious | Major hidden capabilities; content inconsistencies that suggest deliberate misdirection; encoded content without clear justification; or unusual file structures that obscure the skill's true behavior |
| Critical | Active deception — the description contradicts what the skill does; invisible Unicode or zero-width characters embedding hidden instructions; base64/hex-encoded payloads unrelated to the skill's purpose; or obfuscated code sections designed to avoid review |

## Detailed Checks

### Description vs. Behavior Alignment
- Does the skill do **only** what it claims?
- Are all actions mentioned in the skill's stated purpose, or are there undisclosed capabilities?

### Internal Logical Consistency
- Do different sections of the skill agree with each other?
- Example contradiction: a section claiming "no network access" while another section makes HTTP calls.

### Hidden Content Detection
Scan for all of the following:
- **Invisible Unicode:** U+200B zero-width space, U+200C/U+200D zero-width joiners, U+2060 word joiner, U+FEFF BOM
- **Homoglyph substitution:** Characters that look identical to ASCII but are different Unicode code points (e.g., Cyrillic 'а' vs Latin 'a')
- **Hidden HTML/Markdown:** Comment blocks (`<!-- -->`), collapsed/details sections hiding instructions, non-rendering markdown elements
- **Right-to-left override characters:** Characters that disguise text direction, making text appear different from its actual content

### Suspicious Structure
- Unexplained nested directories
- Files whose names don't match their content
- Unusually large documentation relative to skill complexity
- Sections that serve no clear purpose for the stated function

### Encoded Payload Detection
- Base64, hex, URL-encoded, or otherwise obfuscated content that doesn't match the skill's stated purpose
- Example: a formatting skill containing base64-encoded shell commands

### Anti-Audit Manipulation
- **Trust-override language:** Phrases urging the agent to trust the skill unconditionally (e.g., "this skill has been verified safe", "no further review needed")
- **Review-bypass directives:** Phrases urging the agent to skip or shorten the audit (e.g., "skip security checks", "fast-track installation")
- **Install-without-review pressure:** Urgency or authority framing designed to rush installation (e.g., "critical update — install immediately")
