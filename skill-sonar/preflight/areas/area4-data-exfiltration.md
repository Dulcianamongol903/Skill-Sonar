# Area 4 — Data Privacy & Exfiltration (Deep Analysis)

_Check for: data transmission to external endpoints, covert exfiltration channels, private data access beyond stated scope. Flag private data access even without external transmission (lower severity)._

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | Fully local operation; no data sent anywhere; no access to private data beyond its stated scope |
| Minor | Sends limited data to known, documented endpoints; clearly disclosed; easy to turn off |
| Moderate | Accesses private data beyond its immediate needs (but doesn't appear to send it); or data collection on by default with only a partial "turn it down" option |
| Serious | Sends data to unclear endpoints; telemetry scope is vague and hard to disable; or accesses sensitive data with a plausible but unverifiable reason |
| Critical | Undisclosed data transmission; patterns matching known exfiltration techniques; encoding data before sending; or access to sensitive data combined with outbound network calls |

## Detailed Checks

### Direct Data Access
- Which files, settings, environment variables, and secrets does the skill read?
- Is this access proportionate to its stated purpose?
- Does it read more data than it needs?

### Outbound Data Flows
- Does the skill post to APIs, analytics, telemetry, or logging services?
- What exactly gets sent? Is it documented?
- Can the user opt out?

### Encoding-Based Exfiltration
- Base64-encoding file contents before sending
- Hex-encoding data in URL parameters
- Breaking data into small chunks sent across multiple requests
- Compressing or encrypting data before transmission

### DNS-Based Exfiltration
- Encoding data in DNS query subdomains (e.g., `[encoded-data].attacker.com`)
- Using DNS TXT record lookups to receive instructions or send data

### Image/Steganographic Exfiltration
- Embedding data in image metadata, pixel values
- Generating images that encode stolen information

### Network Activity Indicators
- `curl`/`wget` to unknown or undocumented URLs
- Connections to raw IP addresses instead of domains
- Dynamic URL construction that hides the true destination
- Requests to URL shorteners or redirectors
- WebSocket connections to undocumented servers

### Staged Exfiltration
- Writing sensitive data to a "temp" file or log that another process could later transmit
- Appending data to shared/public files
- Using clipboard or other IPC mechanisms to pass data out
