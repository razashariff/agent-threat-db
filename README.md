# Agent Threat Database

Open, community-driven threat intelligence for AI agents and MCP servers. OSV-compatible schema extended for agent-specific threats.

Like [OSV.dev](https://osv.dev) for packages, but for AI agents.

## Why

- 1 in 8 AI breaches now involve autonomous agents (340% YoY growth)
- No CVE-like system exists for malicious agent identities
- No revocation list exists for compromised agent keys
- Package CVEs track the vulnerability, not the agent exploiting it

## Schema

Follows the [OSV schema](https://ossf.github.io/osv-schema/) with agent-specific extensions. Any tool that reads OSV can read this.

```json
{
  "id": "ATD-2026-0001",
  "summary": "...",
  "details": "...",
  "modified": "2026-03-29T00:00:00Z",
  "published": "2026-03-29T00:00:00Z",
  "severity": [{ "type": "CVSS_V3", "score": "..." }],
  "affected": [{ ... }],
  "references": [{ ... }],
  "agent_threat": { ... }
}
```

The `agent_threat` extension adds agent-specific fields not covered by OSV.

## Threat Categories

| Category | Description |
|----------|-------------|
| `data_exfiltration` | Agent extracts and sends data to unauthorised endpoints |
| `credential_theft` | Agent steals API keys, tokens, SSH keys, or secrets |
| `sandbox_escape` | Agent breaks out of execution sandbox |
| `tool_poisoning` | Malicious modification of tool definitions |
| `prompt_injection` | Hidden instructions that hijack agent behaviour |
| `supply_chain` | Compromised package or dependency used by agents |
| `shadow_agent` | Agent spawns persistent sub-agents without user knowledge |
| `unauthorised_action` | Agent performs actions beyond its authorised scope |
| `model_backdoor` | Poisoned model weights with hidden behaviour triggers |

## Entries

All entries are in the `/entries/` directory as individual JSON files following the schema.

## Contributing

Report a malicious agent or incident via GitHub Issues. Include:
- What happened
- Which agent/tool/server was involved
- Evidence (logs, hashes, CVE references)
- Severity assessment

## License

CC-BY-4.0

## Maintained by

[CyberSecAI Ltd](https://cybersecai.co.uk)
