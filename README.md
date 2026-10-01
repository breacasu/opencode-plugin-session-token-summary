# OpenCode Session Token Summary

[![npm version](https://img.shields.io/npm/v/opencode-plugin-session-token-summary)](https://www.npmjs.com/package/opencode-plugin-session-token-summary)

An OpenCode TUI plugin that adds a compact session-usage panel to the sidebar.
It aggregates the root session and all nested subagent sessions.

![alt text](image.png)

## Features

- Compact OpenCode sidebar panel with live session updates
- Input, output, reasoning, cache read, cache write, and total token counts
- Cache hit percentage (or click-to-toggle multiplier) and assistant turn count
- Usage aggregated across the root session and every nested subagent
- Reported API costs for metered providers
- API-equivalent cost estimates for quota and subscription-backed providers
- One combined cost total for sessions that mix metered and quota-backed models
- Clear labels distinguishing reported, estimated, and partially estimated costs
- V2 model-catalog pricing with context-tier support

## Compatibility

| OpenCode version | Plugin version | Installation |
| --- | --- | --- |
| OpenCode 2 | `0.5.0` and later | V2 API; tested with OpenCode 2.0.20. |
| OpenCode V1 (`opencode`) | `0.4.1` | Install the pinned legacy package with the V1 instructions below. |

## OpenCode 2 Install

For a local checkout, add the plugin directory to `~/.config/opencode/cli.json`:

```json
{
  "plugins": [
    "/absolute/path/to/opencode-plugin-session-token-summary"
  ]
}
```

OpenCode 2 resolves the conventional `tui.tsx` entrypoint from the plugin
directory. A direct file path is not loaded by the tested OpenCode 2.0.20
plugin resolver. The published `0.4.x` package targets the legacy TUI plugin
API and cannot be used with OpenCode 2.

The cache row defaults to the usual hit rate: cache reads divided by cache
reads plus input and cache writes. Click the `⇄` cache row to switch between
percentage and the previous `×` multiplier for the current UI session. The
choice stays in effect across sidebar and session re-renders until the plugin
is reloaded. When no tokens have been counted, either view shows `—`;
all-cache-read usage shows `100.0%` or `∞` in multiplier mode.

## OpenCode V1 Install

OpenCode V1 users must install the final legacy release, `0.4.1`:

1. Press `Ctrl+P` in OpenCode and choose **Install plugin**.
2. Press `Tab` to install globally.
3. Enter `opencode-plugin-session-token-summary@0.4.1`.

This creates an entry in `~/.config/opencode/tui.json`. Do not install an
unversioned package in V1 after `0.5.0` is published, because later releases
target OpenCode 2.

## Notes

The panel uses the V2 session-family and message caches to aggregate descendant
usage and count assistant turns.

Nonzero cost reported by OpenCode is shown as `$ cost`. For messages with an
explicit zero cost, the plugin uses OpenCode's model
catalog to calculate what the same tokens would cost at published API rates.
Fully estimated totals are shown as `$ est. cost`; totals that combine reported
and estimated amounts are shown as `$ cost incl. est.` Quota or subscription
users are not necessarily charged the estimated amount. If catalog pricing
cannot be matched, only the reported cost is shown.

## Development

OpenTUI 0.5.12 requires Node >=26.4.0 or Bun >=1.3.0. Local installs with npm
under an older Node version may report `EBADENGINE` or fail when `engine-strict`
is enabled. This release was checked with Bun 1.3.14; OpenCode supplies the TUI
runtime, so users do not need to install Node 26 just to run the plugin.

```sh
npm install
npm run check
npm run pack:check
```

## License

[MIT](LICENSE)
