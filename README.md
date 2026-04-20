# Talk to Figma MCP · Effects Fork

> Fork of [**sonnylazuardi/cursor-talk-to-figma-mcp**](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp) with 3 new tools — **gradient fills, drop shadows, and layer blur** — for AI-native / glassmorphism UI design.

---

## 中文简介

这是 [**cursor-talk-to-figma-mcp**](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp) 的一个分支版本,在原版基础上新增了 3 个工具,让 AI 可以在 Figma 里绘制**渐变、阴影和模糊**这些现代 UI 常用的视觉效果。

**为什么要做这个分支?** 原版 MCP 很适合画线框图,但只支持纯色填充和描边。当我尝试用 AI 还原一个 Hi-Fi 的 Figma 原型时,发现所有的 `box-shadow`、`radial-gradient(...)`、玻璃拟态效果都变成了扁平的方块。Figma 本身是支持这些效果的,只是原版 MCP 没有把对应的 API 暴露出来。所以我加了 3 个工具把这些能力补上。

**新增的工具:**
- **`set_gradient_fill`** — 给任意节点添加线性 / 径向 / 角度 / 菱形渐变,支持 2+ 色标,线性渐变支持角度控制
- **`set_drop_shadow`** — 给节点添加投影或内阴影,可以设置偏移、模糊、扩散、RGBA 颜色,不会覆盖已有的模糊效果
- **`set_layer_blur`** — 给节点添加图层模糊或背景模糊(毛玻璃效果),传入 `radius: 0` 可以移除,不会覆盖已有的阴影效果

**适合什么人用?** 想让 AI 自动生成 Claude / Linear / Anthropic 官网那种带渐变 hero 背景、柔和阴影、玻璃拟态卡片的 Hi-Fi 设计稿的人。安装方法见下方 Install 章节。

**与上游的关系:** 我已经向上游作者提了 Pull Request(见文末 "Relation to upstream" 一节)。如果上游合并了,这个分支就不再需要;如果没有合并,这个分支会继续维护。

---

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

**Upstream PR:** [grab/cursor-talk-to-figma-mcp#170](https://github.com/grab/cursor-talk-to-figma-mcp/pull/170) — open, waiting for review. Once merged, this fork becomes unnecessary.

## Credits

- Original project: [sonnylazuardi/cursor-talk-to-figma-mcp](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp) — Sonny Lazuardi and contributors
- This fork: 3 new effect tools added

Original README preserved as [UPSTREAM_README.md](UPSTREAM_README.md).

## License

MIT — same as upstream. See [LICENSE](LICENSE).
