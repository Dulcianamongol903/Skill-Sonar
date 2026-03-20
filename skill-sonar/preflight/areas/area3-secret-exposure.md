# Area 3 — Secret & Credential Exposure (Deep Analysis)

_Check for: credential access, hardcoded secrets, sensitive file references, secret patterns in code._

## Severity Rubric

| Rating | What It Looks Like |
|--------|-------------------|
| No Concern | No credential access, no references to sensitive files, no secret patterns detected |
| Minor | Reads a single documented config file for a clearly necessary purpose (e.g., a deployment skill reading a deploy config); no secrets embedded |
| Moderate | Accesses environment variables that may contain secrets without documenting which ones and why; or references sensitive file paths without clear justification |
| Serious | Directly accesses known credential stores, secret files, or API key locations; or contains patterns matching known secret formats without explanation |
| Critical | Hardcoded secrets in the skill itself; reads credentials and passes them to external services; accesses multiple credential stores beyond its stated purpose; or credential harvesting patterns |

## Credential & Secret Patterns to Scan

The following specific patterns must be checked against all artifacts:

- **AWS Access & Secret Keys** — `AKIA[0-9A-Z]{16}`, 40-character base64 strings near AWS context
- **OpenAI API Keys** — `sk-[a-zA-Z0-9]{20,}`
- **GitHub Personal Access Tokens** — `ghp_[a-zA-Z0-9]{36}`, `github_pat_` prefixes
- **GitLab Access Tokens** — `glpat-[a-zA-Z0-9-]{20,}`
- **PEM / SSH Private Keys** — `-----BEGIN (RSA|DSA|EC|OPENSSH) PRIVATE KEY-----`
- **Stripe API Keys** — `sk_live_[a-zA-Z0-9]{24,}`, `sk_test_`
- **SendGrid API Keys** — `SG.[a-zA-Z0-9-_]{22}.[a-zA-Z0-9-_]{43}`
- **Twilio Auth Tokens** — 32-character hex strings near Twilio context
- **Slack Bot Tokens & Webhooks** — `xoxb-`, `xoxp-`, `hooks.slack.com/services/`
- **JWT Tokens** — `eyJ[a-zA-Z0-9-_]+.eyJ[a-zA-Z0-9-_]+.`
- **Database Connection URLs** — `mysql://`, `postgres://`, `mongodb://`, `redis://` with embedded credentials
- **Basic Auth Headers** — `Authorization: Basic [base64-encoded]`
- **Generic High-Entropy Strings** — Strings with Shannon entropy > 4.5 in assignment context

## Sensitive File References

Flag any access to:

- `~/.ssh/`, `~/.aws/`, `~/.config/`, `~/.gnupg/`
- `.env`, `.env.local`, `.env.production`, `.env.*`
- `credentials.json`, `service-account.json`, `keyfile.json`
- Browser cookie stores, password managers, keychain files
- `~/.netrc`, `~/.npmrc` (may contain auth tokens), `~/.pypirc`
- `~/.docker/config.json`, `~/.kube/config`
