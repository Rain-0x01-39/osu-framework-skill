# Where to look

Task-indexed pointers into the upstream corpus. Read the listed source **before** guessing an API.

Paths are repo-relative: `wiki` = `github.com/ppy/osu-framework/wiki`, `framework` =
`github.com/ppy/osu-framework`, `tests` = `framework/osu.Framework.Tests`. Use a local clone if present,
otherwise fetch from GitHub.

| Want to… | Wiki page | Local example / source |
|---|---|---|
| Start a project / understand layout | `Setting-up-your-first-project.md` | `framework/SampleGame/` |
| Layout, anchoring, relative sizing | `Coordinate-and-Layout-System.md` | `tests/Visual/Layout`, `tests/Visual/Containers` |
| Animation / transforms | `Transforms-and-tweening.md` | `tests/Visual/Drawables` |
| Observable values (Bindable) | `Bindable-Flow.md` | `tests/Visual/Bindables` |
| Dependency injection / `[Cached]` | `Dependency-Injection.md` | `framework/osu.Framework/Allocation/` |
| Input, click, hover, drag | `Handling-input-events.md` | `tests/Visual/Input` |
| Audio playback | `Playing-audio.md` | `tests/Visual/Audio` |
| Screens and navigation | `Screens-and-Screen-Stacks.md` | `framework/osu.Framework/Screens/` |
| Textures / fonts / resource stores | `Setting-Up-Compiled-Resource-Stores.md`, `Setting-Up-Fonts.md` | `tests/Visual/Sprites`, `tests/Visual/UserInterface` |
| Visual testing / `TestScene` steps | `Development-and-Testing.md` | `tests/Visual/Testing` |
| Async / background loading | `Asynchronous-loading.md` | — |
| Keyboard bindings | `Framework-Key-Bindings.md` | `framework/osu.Framework/Input/Bindings/` |
| Common how-do-I questions | `Common-Scenarios.md` | — |

## Finding an API without a table row

Prefer, in order:
1. `tests/Visual/<topic>/` — runnable usage of almost every public API.
2. `framework/SampleGame/` — smallest end-to-end game.
3. The framework source for the type (e.g. `Drawable.cs`, `Container.cs`).
4. Wiki page.

Grep the framework source for the API name rather than assuming a signature.
