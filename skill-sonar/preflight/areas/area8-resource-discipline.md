# Area 8 — Resource Discipline (Deep Analysis)

_Could this skill run up your costs or slow your system?_

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | All loops and retries have clear limits; no watchers or polling |
| Minor | Retry logic with reasonable caps; minor background activity |
| Moderate | Retries that get broader on failure; watchers that could trigger more work |
| Serious | High potential for runaway API calls or compute use |
| Critical | Unbounded loops, self-triggering scans, or patterns that could cause serious unexpected costs |

## Detailed Checks

### Loop & Recursion Bounds
- Are all loops bounded with clear termination conditions?
- Is recursion depth limited?
- Are there any `while true` or equivalent patterns without break conditions?
- Could any loop grow its own workload (e.g., processing generates more items to process)?

### Retry Logic
- Do retries have maximum attempt limits?
- Is there backoff (exponential or otherwise) between retries?
- Could retries broaden their scope on failure (e.g., retrying with more data, wider search)?
- Are there nested retries that could multiply?

### Watchers & Polling
- File watchers that trigger on their own output (feedback loops)
- Polling intervals — are they reasonable? Do they back off?
- Event listeners that could cascade

### API & Cost Exposure
- Could the skill trigger unbounded API calls (e.g., paginating through unlimited results)?
- Are there LLM/AI API calls in loops?
- Could the skill cause unexpected cloud service charges?
- Token consumption patterns — does the skill process large amounts of text through AI APIs?

### Compute & Memory
- Large file processing without streaming
- In-memory collection of unbounded data
- CPU-intensive operations without limits (e.g., recursive directory traversal of huge trees)
- Spawning processes without limits
