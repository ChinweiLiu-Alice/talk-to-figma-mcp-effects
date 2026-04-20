# Changelog

## [0.4.0] — 2026-04-20 · Effects fork

Forked from [sonnylazuardi/cursor-talk-to-figma-mcp@0.3.5](https://github.com/sonnylazuardi/cursor-talk-to-figma-mcp).

### Added

- **`set_gradient_fill`** — apply linear / radial / angular / diamond gradients to any node that supports `fills`. Accepts 2+ color stops with positions, and an optional angle for linear gradients. Maps to Figma's `GRADIENT_LINEAR | GRADIENT_RADIAL | GRADIENT_ANGULAR | GRADIENT_DIAMOND` paint types.

- **`set_drop_shadow`** — add a drop shadow (default) or inner shadow effect. Takes `offsetX / offsetY / radius / spread` plus RGBA color. Preserves existing blur effects on the node.

- **`set_layer_blur`** — apply a `LAYER_BLUR` or `BACKGROUND_BLUR` (frosted glass) effect. Pass `radius: 0` to remove. Preserves existing shadow effects.

### Changed

- `package.json` renamed to `talk-to-figma-mcp-effects` (v0.4.0) to avoid npm name clash. Adjust if publishing under a different name.

### Unchanged

- All existing tools (40+) work exactly as before.
- Figma plugin structure (`src/cursor_mcp_plugin/`) — just adds 3 command handlers at the bottom.
- Build system (`tsup`), deps, configs.

### Files modified from upstream

```
src/talk_to_figma_mcp/server.ts
  · +3 tool registrations after set_stroke_color
  · +3 entries in FigmaCommand union
  · +3 entries in CommandParams type

src/cursor_mcp_plugin/code.js
  · +3 cases in handleCommand switch
  · +3 new async handler functions:
      setGradientFill(params)
      setDropShadow(params)
      setLayerBlur(params)

package.json
  · name renamed
  · version bumped to 0.4.0

README.md
  · rewritten to explain the fork; original preserved as UPSTREAM_README.md

CHANGELOG.md  (new)
```

Net additions: ~280 lines across server + plugin. No breaking changes.
