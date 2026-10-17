# SLOPCANNON plugin for Grok Bot and Cursor

> **Unofficial community plugin.** It is not made, endorsed, or supported by SLOPCANNON / slop.cc unless they confirm otherwise. Use it at your own risk; all generation is billed to your own slop.cc account.

[SLOPCANNON](https://slop.cc) is an AI music-video generator: give it a song, optional character references, and a concept, and it produces a lip-synced, scene-by-scene music video. This plugin connects your agent to SLOPCANNON's hosted MCP server and adds a director skill with safety and spend guardrails.

## What's inside

| Path | What it is |
| --- | --- |
| `mcp.json`, `.mcp.json` | Remote HTTP MCP server `https://api.slop.cc/api/mcp`, Bearer token auth |
| `skills/slopcannon-music-video/` | End-to-end workflow: brief, rights, plan, quote, scene 1, rest, assemble |
| `commands/slopcannon.md` | `/slopcannon`: getting started and natural-language entrypoint |
| `.grok-plugin/`, `.cursor-plugin/` | Plugin manifests |

## Install

**Grok Bot / Cursor:** add this repository as a plugin (for example, a plugin marketplace entry or a local plugin pointing at a clone of this repo), then enable it.

```bash
git clone https://github.com/lalomorales22/slopcannon-grok-plugin.git
```

## Token setup

1. Sign in at https://slop.cc/account and create an API token.
2. Give the plugin that token as `SLOPCANNON_API_TOKEN`, using the plugin's secret/setup field or an environment variable visible to your agent host:
   ```bash
   export SLOPCANNON_API_TOKEN=...   # don't commit this
   ```
3. The MCP config sends it as `Authorization: Bearer ${SLOPCANNON_API_TOKEN}`. Nothing is stored in this repo.

Never paste your token into chat. The skill tells the agent never to print, log, or request it.

## Guardrails built in

- **Rights:** before any upload, the agent shows SLOPCANNON's rights statement verbatim and waits for *you*, the owner, to explicitly confirm you have the rights to the song and images.
- **Spend:** every scene gets a server quote. The agent shows the total and waits for your approval (or a cap you set).
- **Scene 1 first:** one scene is generated and reviewed before the rest are submitted.
- **Never publishes:** the agent delivers a download link. Sharing is up to you.

Pricing (check the live manifest): 8 credits per second, 40-credit minimum per generation, 5 to 15 s scenes, one scene per quote.

## Example prompts

- `/slopcannon` (guided setup and first video)
- "Make a 9:16 music video for this track: two cats run a late-night diner, chaos builds to a food fight."
- "Quote a 45-second video at the default model before I commit."
- "Scene 4's take has text artifacts; reroll just that scene, cap 120 credits."
- "Assemble my selected takes and give me the download link."

## License

MIT. See [LICENSE](LICENSE). SLOPCANNON and slop.cc are the property of their respective owners.
