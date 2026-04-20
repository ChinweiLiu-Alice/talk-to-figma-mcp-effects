# Talk to Figma MCP · Effects Fork

> Fork of [**sonnylazuardi/cursor-talk-to-figma-mcp**](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp) with 3 new tools — **gradient fills, drop shadows, and layer blur** — for AI-native / glassmorphism UI design.

## Why this fork exists

Upstream `cursor-talk-to-figma-mcp` is great for structural wireframes, but it only exposes **solid fills and strokes**. Modern AI product UIs (think Claude, Linear, Anthropic.com, most AI SaaS) lean heavily on:

- Radial / linear gradients for hero backgrounds and brand badges
- Soft colored drop shadows for elevation
- Background blur for glassmorphism cards

When I tried to recreate a Hi-Fi Figma prototype from an HTML design spec, every `box-shadow: 0 8px 18px #E3121F33` and `radial-gradient(...)` turned into a flat rectangle. So I added three tools to fix it.

**Figma supports all of this natively** — the original MCP wrapper just didn't expose the APIs. These tools are thin wrappers around `node.fills = [{type: 'GRADIENT_LINEAR', ...}]` and `node.effects = [...]`.

## What's new

| Tool | What it does |
|------|--------------|
| **`set_gradient_fill`** | Apply linear / radial / angular / diamond gradient to any node. 2+ color stops. Angle control for linear. |
| **`set_drop_shadow`** | Add drop shadow or inner shadow. Offset, blur radius, spread, color + alpha. Preserves existing blur effects. |
| **`set_layer_blur`** | Apply layer blur OR background blur (frosted glass). Radius 0 removes. Preserves shadows. |

### Examples

```js
// Radial gradient hero background
set_gradient_fill({
  nodeId: "1:2",
  type: "radial",
  stops: [
    { position: 0, r: 1, g: 0.67, b: 0.97 },
    { position: 0.5, r: 0.6, g: 0.39, b: 1 },
    { position: 1, r: 0.2, g: 0.37, b: 1 }
  ]
})

// Brand-colored button shadow
set_drop_shadow({
  nodeId: "1:3",
  offsetX: 0, offsetY: 12, radius: 24,
  r: 0.89, g: 0.07, b: 0.122, a: 0.35
})

// Frosted glass card
set_layer_blur({
  nodeId: "1:4",
  radius: 20,
  background: true  // BACKGROUND_BLUR — peeks through to content below
})
```

## Install

### Option A: Run from source (recommended while this fork is unpublished)

```bash
git clone https://github.com/<your-username>/talk-to-figma-mcp-effects.git ~/mcp-forks/talk-to-figma-mcp-effects
cd ~/mcp-forks/talk-to-figma-mcp-effects
bun install
bun run build
```

Point your Claude / Cursor MCP config at the local build:

```json
{
  "mcpServers": {
    "TalkToFigma": {
      "command": "bun",
      "args": [
        "run",
        "/Users/<you>/mcp-forks/talk-to-figma-mcp-effects/dist/server.js"
      ]
    }
  }
}
```

### Load the Figma plugin

In Figma desktop:
1. `Plugins` → `Development` → `Import plugin from manifest…`
2. Pick `src/cursor_mcp_plugin/manifest.json` from this repo
3. Run the plugin, click **Connect**, copy the channel name back to the AI

### Run the WebSocket bridge

```bash
bun run src/socket.ts
# or
bun socket
```

### Restart Claude / Cursor

The 3 new tools become visible in your AI agent's tool list after restart.

## Compatibility

- Drop-in superset of upstream — all existing tools still work identically
- Existing `.mcp.json` configs continue to function after re-pointing `command/args` to the new server path
- Figma plugin code is in `src/cursor_mcp_plugin/` — same structure as upstream, just with 3 extra command handlers

## Relation to upstream

This is a friendly fork. If the upstream maintainer accepts it, these 3 tools should live there. See `CHANGELOG.md` for the exact diff.

Pull request welcome — if you're reading this and you're `@sonnylazuardi`, I'd be happy to upstream this.

## Credits

- Original project: [sonnylazuardi/cursor-talk-to-figma-mcp](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp) — Sonny Lazuardi and contributors
- This fork: 3 new effect tools added

Original README preserved as [UPSTREAM_README.md](UPSTREAM_README.md).

## License

MIT — same as upstream. See [LICENSE](LICENSE).
