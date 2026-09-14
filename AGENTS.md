# Repository Guide

## Run And Verify

- This is a dependency-free browser game: there is no package manifest, build step, test suite, linter, or formatter configuration.
- Run it from the repository root with `npx serve .`, then open `http://localhost:3000`. Opening `index.html` directly also works; use the server when browser tooling requires an HTTP origin.
- Use `node --check game.js` for the only available automated syntax check.
- After gameplay changes, manually verify movement (`ArrowLeft`, `ArrowRight`, `ArrowUp`), shooting (`Space`), asteroid splitting/scoring, life loss/respawn, level advancement, and game-over restart (`Space`).

## Runtime Shape

- `index.html` is the sole page and loads `game.js` as a classic script, not an ES module. `game.js` initializes global state immediately and starts the `requestAnimationFrame` loop at file end.
- Keep the canvas dimensions in `index.html` (`800x600`) synchronized with `W` and `H` in `game.js`; physics and wrapping use the JavaScript constants.
- The frame delta passed to `update` is in seconds and capped at `0.05`. Movement, cooldown, and lifetime constants are therefore expressed per second.
- Input has two semantics: `keys` tracks held controls, while `pressed()` consumes a one-frame edge. `Space` uses the consumed edge for both shooting and restarting after game over.
- Ship, bullets, and asteroids wrap at screen edges, but collision checks use ordinary Euclidean distance. Objects visually adjacent across opposite edges do not collide.

## Source Of Truth

- Treat `game.js` as authoritative for implemented behavior. The README currently mentions power-ups and a shooting-star asteroid, but neither exists in the code.
- All game entities, mutable state, update/collision logic, and drawing live in `game.js`; there are no package or generated-code boundaries to preserve.
