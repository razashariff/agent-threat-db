# Agent Threat Database Schema

Based on [OSV Schema v1.6.7](https://ossf.github.io/osv-schema/) with `agent_threat` extension.

## Base Fields (OSV-compatible)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | Yes | Unique identifier (ATD-YYYY-NNNN) |
| `summary` | string | Yes | One-line description (max 120 chars) |
| `details` | string | Yes | Extended description (CommonMark) |
| `modified` | timestamp | Yes | Last modification (RFC 3339) |
| `published` | timestamp | Yes | Publication date (RFC 3339) |
| `withdrawn` | timestamp | No | Withdrawal date if retracted |
| `aliases` | string[] | No | Cross-references (CVE-*, GHSA-*) |
| `severity` | object[] | No | CVSS scoring |
| `affected` | object[] | Yes | Affected packages/agents |
| `references` | object[] | No | Links to advisories, articles, fixes |
| `credits` | object[] | No | Who reported/discovered |

## Extension: `agent_threat`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `category` | string | Yes | Threat category (see categories) |
| `agent_type` | string | Yes | `mcp_server`, `mcp_client`, `autonomous_agent`, `agent_skill`, `model` |
| `attack_vector` | string | No | How the attack works |
| `key_fingerprints` | string[] | No | SHA-256 fingerprints of compromised agent keys |
| `iocs` | object[] | No | Indicators of compromise (IPs, domains, hashes) |
| `mitigations` | string[] | No | How to protect against this threat |
| `mcps_detection` | object | No | How mcp-secure detects this (signTool, verifyModel, etc.) |

## Example Entry

```json
{
  "id": "ATD-2026-0001",
  "summary": "mcp-remote OS command injection enables credential theft in 437K environments",
  "details": "mcp-remote versions prior to 1.7.0 are vulnerable to OS command injection...",
  "modified": "2026-03-29T00:00:00Z",
  "published": "2025-07-09T00:00:00Z",
  "aliases": ["CVE-2025-6514", "GHSA-6xpm-ggf7-wc3p"],
  "severity": [
    { "type": "CVSS_V3", "score": "CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N" }
  ],
  "affected": [
    {
      "package": {
        "ecosystem": "npm",
        "name": "mcp-remote"
      },
      "ranges": [
        {
          "type": "SEMVER",
          "events": [
            { "introduced": "0" },
            { "fixed": "1.7.0" }
          ]
        }
      ]
    }
  ],
  "references": [
    { "type": "ADVISORY", "url": "https://research.jfrog.com/vulnerabilities/mcp-remote-command-injection-rce-jfsa-2025-001290844/" },
    { "type": "FIX", "url": "https://github.com/geelen/mcp-remote/commit/607b226a" }
  ],
  "agent_threat": {
    "category": "credential_theft",
    "agent_type": "mcp_server",
    "attack_vector": "Crafted authorization_endpoint URL triggers OS command injection during OAuth flow",
    "iocs": [],
    "mitigations": [
      "Upgrade mcp-remote to >= 1.7.0",
      "Use mcp-secure verifyTool() to detect modified server definitions",
      "Scan with cybersecify before connecting to MCP servers"
    ],
    "mcps_detection": {
      "function": "verifyTool",
      "detects": "Modified tool definitions after server compromise"
    }
  }
}
```
