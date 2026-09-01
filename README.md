# Spara for Claude

Connect Claude to your Spara workspace to inspect and edit GTM agents as drafts,
analyze your data, and search Spara documentation. Access is authenticated with
Spara and scoped to the organization you select.

## Install

### Claude app and Cowork

1. Open **Settings → Plugins → Add marketplace**.
2. Enter `https://github.com/spara-ai/spara-plugins`.
3. Install **Spara**.
4. Complete the Spara sign-in flow and select your organization.

### Claude Code

Run these commands in Claude Code:

```text
/plugin marketplace add spara-ai/spara-plugins
/plugin install spara@spara
```

Then run `/mcp` and complete the browser sign-in for the `spara` connector.
There is no client ID, client secret, or API key to paste.

## Use Spara

Ask Claude questions such as:

- "What agents and channels do we have set up in Spara?"
- "In Spara, how are our chat conversions trending over the last month?"
- "Using Spara, tighten the opening line of our chat agent's instructions."
- "What lead fields does Spara store, and which sync from our CRM?"
- "How do I set up a product demo agent in Spara?"

The plugin can:

- Read agents, channels, configuration, data-model fields, and connected CRM
  fields.
- Answer natural-language analytics questions about your organization's data.
- Create agents and channels or edit channel configuration. Every change is
  saved as an unpublished draft for a person to review and publish in Spara.
- Search and answer questions from Spara's product documentation.

The plugin cannot publish a configuration change. It does not expose raw SQL or
raw analytics rows.

## Requirements and access

- A Spara customer account and membership in the organization you select.
- Claude Code or a Claude surface that supports plugins.
- The `org:agent:edit` Spara permission for configuration changes. Users without
  it receive the read-only tool set.

Spara resolves identity, organization membership, and permissions on every tool
call. Claude does not choose or send an organization identifier.

## Troubleshooting

- **`spara` is not connected:** run `/reload-plugins`, then `/mcp`, and complete
  sign-in.
- **Calls are denied after sign-in:** reconnect with an organization where you
  are an active member and have the permission required by the requested action.
- **An update is missing:** run `/plugin marketplace update spara`, then update
  **Spara** from `/plugin`.
- **Installation fails:** confirm the marketplace is exactly
  `spara-ai/spara-plugins`, then retry the two Claude Code install commands.

## Support and security

- Product help and installation issues: [support@spara.com](mailto:support@spara.com)
- Documentation: [docs.spara.com](https://docs.spara.com)
- Privacy: [Spara Privacy Statement](https://www.spara.com/privacy)
- Security vulnerabilities: follow [SECURITY.md](SECURITY.md) and report them
  privately rather than opening a public issue.

See [CONTRIBUTING.md](CONTRIBUTING.md) for feedback and contribution guidance.

## License

Licensed under the [Apache License 2.0](LICENSE).
