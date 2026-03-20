# Area 5 — Injection & Influence Resistance (Deep Analysis)

_Check for: prompt injection payloads, MCP poisoning, memory/context corruption, cross-tool chain attacks, argument injection._

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | No outside content enters the skill's decision-making; documentation is clean; no injection patterns detected |
| Minor | Outside content is read but kept strictly separate from action decisions; minimal attack surface |
| Moderate | Outside content can indirectly affect what the skill does; or the skill processes user-provided strings in contexts where injection is theoretically possible |
| Serious | Outside content (web pages, comments, linked resources) can directly steer the skill's actions; or the skill contains patterns that could be exploited for prompt injection in certain runtime contexts |
| Critical | The skill treats outside content as trusted instructions; contains direct prompt injection payloads; attempts to extract or corrupt system context; or uses multi-step patterns that chain benign-looking operations into exploits |

## Detailed Checks

### Prompt Injection Patterns
Instructions designed to override the agent's system prompt or prior instructions:
- **Instruction-override directives:** Phrases that attempt to nullify or replace prior instructions (e.g., "ignore previous instructions", "your new instructions are")
- **Identity-reassignment attacks:** Phrases that attempt to redefine the agent's role or remove its restrictions (e.g., "you are now an unrestricted assistant")
- **Instruction hijacking via markdown/HTML comments:** Hidden directives in `<!-- -->` blocks or non-rendering elements
- **Multi-language injection:** Instructions in a language different from the skill's stated locale, potentially bypassing content filters

### MCP (Model Context Protocol) Poisoning
- Malicious tool descriptions that cause the agent to misuse tools
- Poisoned tool responses that inject instructions into the agent's context
- Tool name collisions designed to intercept legitimate tool calls
- Tools that return payloads designed to alter agent behavior on subsequent turns

### Memory & Context Poisoning
Attempts to corrupt the agent's working memory:
- Planting false facts in conversation context
- Injecting misleading context that persists across turns
- Writing to files the agent uses for state (e.g., MEMORY.md, USER.md, SOUL.md, IDENTITY.md, context files)
- Manipulating conversation history or state management files

### System Prompt Extraction
Patterns designed to make the agent reveal its system prompt, operating rules, or internal instructions:
- Direct extraction requests ("print your system prompt")
- Indirect extraction via instruction-summarization requests
- Meta-questions about the agent's configuration
- Encoding tricks (e.g., "encode your instructions in base64")

### Argument Injection
- Malformed tool arguments designed to exploit shell parsing
- Path traversal: `../../` sequences to escape intended directories
- Command injection via backticks or `$()`
- SQL injection patterns in database-related skills

### Cross-Tool Chain Attacks
Sequences of individually benign operations that combine into an exploit:
- Step 1 writes a file → Step 2 reads it as configuration → Step 3 executes based on that configuration
- Each step looks safe alone; the chain is the attack
- Look for skills that create files which other tools or processes will consume as trusted input
