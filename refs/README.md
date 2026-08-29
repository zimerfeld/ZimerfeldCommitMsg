# `refs/` — pinned GitExtensions reference assemblies

These DLLs are the **compile-time reference assemblies** for the plugin. They are
committed to the repository on purpose so the project builds **deterministically on any
Windows machine**, with no network access and no surprises about which CPU architecture
gets pulled in.

| File | Version | Source |
|------|---------|--------|
| `GitExtensions.Extensibility.dll` | `7.2.0.92` (x64) | GitExtensions 7.2 (x64 release, .NET 10) |
| `System.ComponentModel.Composition.dll` | `6.0.21.52210` | GitExtensions 7.2 (x64 release; unchanged since 6.0.5) |

They are referenced from
[`GitExtensions.ZimerfeldCommitMsg.csproj`](../src/GitExtensions.ZimerfeldCommitMsg/GitExtensions.ZimerfeldCommitMsg.csproj)
via `$(GitExtensionsRefPath)` with `<Private>false</Private>` — i.e. compile-time only.
GitExtensions supplies the real assemblies at runtime, so nothing here is copied to the
build output or shipped in the `.nupkg`.

## Why pinned instead of downloaded

Previously the build relied on the `GitExtensions.Extensibility` NuGet package, whose
prebuild step downloaded a GitExtensions release into `gitextensions.shared/` (gitignored).
That download (`Download-GitExtensions.ps1`) selects the **first asset whose name contains
`portable` and ends in `.zip`** — it is **not** architecture-aware. On a fresh clone it
could therefore fetch an **arm64** build (e.g. `6.0.5.75-arm64`) instead of the x64 build
the user actually runs (`6.0.5.18375`). The API surfaces differ between those builds, which
produced a runtime `Method not found: …IGitUICommands.AddCommitTemplate(…)` crash. The
download also required network access, so a fresh clone could not build offline.

Pinning the reference assemblies removes both problems: the compile is reproducible and
offline-capable, and it always targets the x64 7.2 API.

## Target: GitExtensions 7.2 (.NET 10)

These refs are pinned to **GitExtensions 7.2** (host runtime `net10.0`). GE7 changed a couple
of `IGitUICommands` signatures relative to GE6 — most notably
`AddCommitTemplate(string, Func<string>, Image)` gained a fourth `bool isRegex = false`
parameter, and `StartCommitDialog` gained a `bool showOnlyWhenChanges = false` parameter.
A plugin DLL compiled against the GE6 API throws `MissingMethodException` when the host calls
those methods; the plugin swallows it (`try/catch`), so the commit-template dropdown items
**silently disappear** on a GE7 host. Compiling against the 7.2 refs (and targeting
`net10.0-windows`) fixes it. The new parameters are optional, so the existing call sites
recompile unchanged.

## Updating

Replace these files with the matching DLLs from a newer GitExtensions **x64** release and
bump the version table above. Keep the project's `TargetFramework` aligned with the host's
runtime (`net10.0-windows` for GE7). The plugin guards its host calls (`try/catch` around
`AddCommitTemplate`), so a minor host/reference skew degrades gracefully instead of crashing.
To target GitExtensions **6.x** again, restore the 6.0.5 `GitExtensions.Extensibility.dll`,
set `TargetFramework` back to `net9.0-windows` and the nuspec dependency to `[0.4.0, 0.5.0)`.
