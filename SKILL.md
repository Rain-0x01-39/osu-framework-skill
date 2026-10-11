---
name: osu-framework-skill
description: Build, run, and debug a game on osu!framework (osu.Framework) using C#. Use when scaffolding a new game project from the ppy templates, writing game code with Drawables, Game, ScreenStack, Bindables, dependency injection, transforms, or layout, wiring desktop/Android/iOS heads, or running the visual test browser. Also use when a user who does not write code wants to build or change an osu.Framework game. Do NOT use for contributing to the osu-framework framework itself.
---

# osu!framework Game Development

Helping build a **game that uses** osu!framework. This skill is only about shipping a game on top of the
framework — do not cover contributing to osu-framework itself.

## Scaffold a new project

```bash
dotnet new install ppy.osu.Framework.Templates          # once per machine
dotnet new osu-framework-game -n MyGame                 # empty starter
dotnet new osu-framework-flappy-game -n MyGame          # complete example game
```

Project names must have **no spaces or hyphens** (the templating system breaks on them).

### Generated layout (three projects + resources)

| Project | Role |
|---|---|
| `MyGame.Game` | All game logic. This is where you write code. |
| `MyGame.Desktop` | Desktop launch head (`Program.Main` → host). |
| `MyGame.Game.Tests` | Visual test browser (NUnit + `TestScene`). |
| `MyGame.Resources` | Compiled textures/audio/etc. |

The template also emits `MyGame.iOS` but **no Android head** — for Android, copy the head from the
framework's own `SampleGame.Android` or from ppy/osu.

## Reference corpus — read upstream before inventing APIs

The ground truth is the upstream `ppy/osu-framework` repo and its wiki. If a local clone is present, read
it directly; otherwise fetch from GitHub.

- Source: `github.com/ppy/osu-framework`
  - `SampleGame/` — minimal game (`SampleGameGame.cs`) — read this first.
  - `osu.Framework.Tests/Visual/` — hundreds of `TestScene`s; the best living API examples.
  - `osu.Framework/` — the actual API (Drawable, Bindable, transforms, layout).
- Wiki: `github.com/ppy/osu-framework/wiki`
  - `Setting-up-your-first-project`, `Bindable-Flow`, `Dependency-Injection`,
    `Coordinate-and-Layout-System`, `Transforms-and-tweening`, `Development-and-Testing`.

**Rule:** when unsure how an API is used, read the source or the Tests project — do not guess.

**Quick index:** `references/where-to-look.md` maps common tasks to the wiki page and example to read.

## Workflow

```bash
dotnet build MyGame.sln
dotnet run --project MyGame.Desktop           # run the game
dotnet run --project MyGame.Game.Tests        # open the test browser
```

## Core mental model

- **`Game`** is the root; it owns a `GameHost` (window + renderer + input). You rarely subclass beyond the
  generated `MyGameGame`.
- **`Drawable`** is the unit of everything visible/interactive. `Add`/`AddRange`/`Child`/`Children` build the tree.
- **`ScreenStack`** + `Screen` — push/pop top-level screens.
- **`Bindable<T>`** — observable values that drive UI. Read `Bindable-Flow.md` before touching them.
- **`[BackgroundDependencyLoader]`** — DI injection point; its parameters are resolved from cached dependencies.
- **Transforms** — animations. `this.FadeIn(500)`, `.MoveTo(...)`, `.RotateTo(...)`; chainable via `.Then`.
- **Layout** — `Anchor`/`Origin`/`RelativeSizeAxes`/`RelativePositionAxes`/`AutoSizeAxes`. Read the layout wiki page.
- **`LoadComplete()`** — override to act after children are loaded.

## Testing = visual test browser

Write a `TestScene` subclass with `[TestFixture]`, then `AddStep(...)` / `AddAssert(...)`. The test browser
shows it visually and runs it under NUnit. Use this to iterate fast without running the whole game.

## Pitfalls

- Never block the update thread; use `Scheduler.Add` / async loading, not `Thread.Sleep`.
- Prefer existing framework controls (`Box`, `SpriteText`, `Container`, `FillFlowContainer`) before writing new ones.
- Keep the first version small: a screen with a spinning `Box` proves the pipeline. Grow from there.
- Treat the wiki as **lower priority than the source and Tests** — parts are stale (e.g. the window/renderer page still says SDL2; current code defaults to SDL3 outside macOS). When they disagree, source wins.
