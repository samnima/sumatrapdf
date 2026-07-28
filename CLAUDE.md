# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

SumatraPDF is a multi-format document reader (PDF, EPUB, MOBI, CBZ, CBR, FB2, CHM, XPS, DjVu) for Windows.
It's a C++ program built directly against the Win32 API (no MFC, no WTL). Most code is under
(A)GPLv3, some under BSD (see `AUTHORS`).

Key facts:
- We **don't use STL**. Custom string / container / helper types live in `src/utils` (e.g. `Vec`,
  `StrVec`, string helpers) and are used throughout instead of `std::string`/`std::vector`/etc.
- Assume Visual Studio command-line tools (`cl.exe`, `msbuild.exe`, etc.) are available on `PATH`.
- The repo builds only on Windows. If you're working from a non-Windows shell, you can still edit,
  format, and reason about the code, but you cannot actually compile or run it.

## Repository layout

- `src/` — the application itself (all first-party C++ code).
  - `src/utils/` — base library: strings, containers, file/OS helpers (the "no STL" replacement).
    Has its own unit tests in `src/utils/tests/*_ut.cpp`.
  - `src/wingui/` — thin Win32 UI control wrappers (buttons, edit boxes, tree view, splitter, etc.).
  - `src/mui/` — a small retained-mode UI/text-rendering layer used by parts of the app.
  - `src/uia/` — UI Automation support (accessibility).
  - `src/tools/` — small standalone tool sources (`test_util.cpp`, `MakeLzSA.cpp`, `signfile.cpp`, ...).
  - `src/previewer/`, `src/ifilter/` — Windows shell extensions (thumbnail previewer, search/index filter).
  - `src/regress/`, `src/testcode/` — regression/scratch test code.
  - Top-level `src/*.cpp|h` — the main app: window/tab management, engines glue, settings, commands,
    printing, annotations, command palette, crash handling, etc.
- `mupdf/` — vendored MuPDF, used as the primary rendering engine for PDF/XPS/EPUB/etc.
- `ext/` — other vendored third-party dependencies (zlib, libwebp, freetype, harfbuzz, unarr,
  libarchive, dav1d, libheif, darkmodelib, brotli, ...). Treat these as upstream; avoid modifying
  unless necessary, and prefer minimal, clearly-scoped patches if you must.
- `cmd/` — the build/dev tooling, written as TypeScript run with **bun** (see `cmd/package.json`,
  `cmd/tsconfig.json`). This is the primary interface for building, formatting, generating code,
  and releasing — not raw `msbuild`/`premake5` invocations.
- `premake5.lua`, `premake5.files.lua` — Premake5 build definitions that generate the Visual Studio
  solution/projects checked into `vs2022/`. `bin/premake5.exe` is the vendored premake binary.
- `vs2022/` — generated Visual Studio solution and project files (checked into the repo).
- `docs/md/` — user- and developer-facing documentation (wiki-style .md pages published to the
  SumatraPDF website); `docs/md/Version-history.md` is the changelog.
- `translations/` — translation source files (`translations.txt`, etc.).
- `.github/workflows/` — CI: `build.yml` (build/test), `codeql.yml`, `daily.yml`.

## Build, format, test commands

All tooling is invoked through `bun` from the repo root.

- **Build (debug):** `bun ./cmd/build.ts` → produces `./out/dbg64/SumatraPDF.exe`.
- **Other build variants:** see other `cmd/build-*.ts` scripts (`build-all.ts`, `build-ci.ts`,
  `build-smoke.ts`, `build-with-mingw.ts`, `build-no-info.ts`, `build-codeql.ts`).
- **Regenerate Visual Studio project files** (only needed after adding/removing source files, i.e.
  after editing `premake5.files.lua`): `bun cmd/premake.ts`.
- **Format C/C++ code:** `bun cmd/format.ts` — runs `clang-format -style=file` (Chromium-based style,
  see `.clang-format`) over `src/`, `ext/CHMLib`, and a couple of other whitelisted ext files.
  **Always run this on any `.cpp`/`.c`/`.h` file you touch, before building.**
- **Run unit tests:** `bun cmd/run-tests.ts` — builds the `test_util` project (Release|x64) and runs
  `out/rel64/test_util.exe`. The tests themselves live in `src/utils/tests/*_ut.cpp` (one file per
  utility, e.g. `StrUtil_ut.cpp`, `Vec_ut.cpp`, `WinUtil_ut.cpp`); there's no per-test filter flag,
  the whole `test_util` binary runs each time.
- **Static analysis:** `bun cmd/clang-tidy.ts` (clang-tidy, config in `.clang-tidy`) and
  `bun cmd/cppcheck.ts`. The `ReleaseAnalyze` premake configuration also runs `/analyze` via MSVC.
- **Debug a build:** `` windbgx -Q -o -g ./out/dbg64/SumatraPDF.exe ``
- **Clean:** `bun cmd/clean.ts`.

Build variants/platforms defined in premake: configurations `Debug` / `Release` / `ReleaseAnalyze`;
platforms `Win32`, `x64`, `x64_asan` (ASan, 64-bit only). Per-build `#ifdef` customization that
doesn't warrant its own configuration goes in `src/utils/BuildConfig.h` (empty by default).

### Workflow rules

- After editing any `.cpp`/`.c`/`.h` file and **before** running `build.ts`, run clang-format on
  those files to reformat in place (see Format command above).
- Never commit changes automatically — always wait for an explicit instruction to commit.

## Code generation workflows

Several `src/*.h`/`src/*.cpp` files are generated from `cmd/gen-*.ts` scripts and must not be hand-edited
directly for structural changes — edit the generator input instead and regenerate.

**Adding a new advanced setting:**
1. Add the definition in `cmd/gen-settings.ts`.
2. Run `bun cmd/gen-settings.ts` to regenerate `src/Settings.h` and `src/Settings.cpp`.

**Adding a new command:**
1. Add to `cmd/gen-commands.ts`, always at the end of the list (before the `CmdNone` command).
2. Run `bun cmd/gen-commands.ts` to regenerate `src/Commands.h` and `src/Commands.cpp`.
3. Document it in `docs/md/Commands.md`.
4. Document it in `docs/md/Version-history.md` under the **next** (unreleased) section.

**Adding a new command-line flag:**
1. Add to `cmd/gen-flags.ts`.
2. Run `bun cmd/gen-flags.ts` to regenerate `src/Flags.h` and `src/Flags.cpp`.
3. Implement the handling logic in `Flags.cpp`.
4. Document it in `docs/md/Version-history.md` under the **next** (unreleased) section.

## High-level architecture

**Entry point:** `src/SumatraStartup.cpp` (`WinMain`) — parses command-line flags (`Flags.h/.cpp`,
generated), initializes globals/settings, and creates the first `MainWindow`.

**Window/tab model:** `MainWindow` (`src/MainWindow.h`) represents a top-level application window;
each window holds one or more `WindowTab`s (`src/WindowTab.h`), one per open document. UI chrome
(toolbar, tab strip, table of contents sidebar, canvas) is composed around this.

**Document abstraction — two layers:**
1. `EngineBase` (`src/EngineBase.h`, plus `EngineCreate.cpp`, `EngineAll.h`) is the format-agnostic
   rendering interface implemented per format: `EngineMupdf.cpp` (PDF/XPS/EPUB/etc. via MuPDF),
   `EngineDjVu.cpp`, `EngineImages.cpp` (images/comic archives), `EnginePs.cpp`, `EngineEbook.cpp`
   (MOBI/FB2/etc., not backed by mupdf). `EngineDump.cpp` is a debug/dump utility over engines.
2. `DocController` (`src/DocController.h`) is the higher-level "controller" interface used by the UI
   for navigation/zoom/TOC/display state; it's implemented by `DisplayModel` (`DisplayModel.h/.cpp`,
   for fixed/engine-backed documents — the `AsFixed()` cast target) and `ChmModel` (`ChmModel.h/.cpp`,
   for CHM help files, rendered via an embedded `HtmlWindow`). The UI talks to documents through
   `DocController`/`ILinkHandler`/`DocControllerCallback`, not through concrete model types, so new
   document kinds should plug in at this layer.

**Canvas & rendering:** `Canvas.cpp` (very large) owns the main drawing/paint/scrolling/input-handling
logic for the document view; `DisplayModel` tracks per-page layout/zoom/scroll state feeding it.

**UI toolkit layers:** `src/wingui/` provides thin, direct wrappers over native Win32 controls (used
for dialogs, toolbars, tree views, etc.); `src/mui/` is a separate, smaller retained-mode layer used
for a subset of custom-drawn UI/text. Prefer `wingui` for new native-control UI.

**Settings/commands/flags:** `Settings.h/.cpp`, `Commands.h/.cpp`, `Flags.h/.cpp` are all generated
(see Code generation workflows above) from declarative lists in `cmd/gen-*.ts`. `AppSettings.cpp`
and `GlobalPrefs`-style state build on top of generated `Settings.h`. `Commands.cpp` maps command
IDs to behavior and feeds `CommandPalette.cpp` and menu/accelerator wiring (`Accelerators.cpp`).

**Other notable subsystems:** `CrashHandler.cpp` (crash reporting), `Favorites.cpp`/`FileHistory.cpp`
(recent files/bookmarks), `Annotation.cpp`/`EditAnnotations.cpp` (PDF annotation editing), `Print.cpp`,
`ExternalViewers.cpp` (open-with integration), `ChmFile.cpp` (low-level CHM parsing, distinct from
`ChmModel.cpp` which is the `DocController` implementation).

**Build targets (from `premake5.lua`):** `SumatraPDF` (single static exe — the normal build) and
`SumatraPDF-dll` (same UI code, but most functionality lives in `libmupdf.dll`) share the same source
lists (`sumatrapdf_files()`, `engines_files()`, `wingui_files()`, `mui_files()`, `uia_files()`,
`synctex_files()`). Other projects: `test_util` (unit tests), `PdfFilter`/`PdfPreview` (Windows shell
extensions from `src/ifilter`/`src/previewer`), `MakeLZSA` (translation/resource archive packer used
in the `SumatraPDF` prebuild step), `bin2coff`, plus vendored library projects (`libmupdf`, `unrar`,
`libdjvu`, `libarchive`, `libwebp`, `libheif`, `dav1d`, `zlib`, `chm`, ...).

## Coding conventions

- Style: Chromium-based clang-format profile — 4-space indent, 120 column limit, left-aligned
  pointers (`Foo* p`), left-aligned access modifiers offset by -2. Always run `bun cmd/format.ts`
  rather than hand-formatting.
- C++17/C++latest, no STL containers/strings — use `src/utils` equivalents.
- clang-tidy (`.clang-tidy`) enables `bugprone-*`, `clang-analyzer-*`, `misc-*`, `modernize-*`,
  `performance-*`, `portability-*`, `readability-*` with a curated set of exclusions; `.clangd`
  suppresses several noisy readability/modernize checks in the editor (unused-parameter, magic
  numbers via clang-tidy config, etc.) and excludes `mupdf/` and `ext/` from diagnostics entirely.
- Copyright header convention: new source files start with
  `/* Copyright <year> the SumatraPDF project authors (see AUTHORS file).\n   License: ... */`
  matching the file's license (GPLv3 vs Simplified BSD — check neighboring files in the same
  directory for which applies).

## Windows shell safety (important for the Bash tool)

The Bash tool runs under Git Bash (MSYS2), **not** `cmd.exe`. This causes issues with Windows-style
commands:
- **Never use `2>nul`** — Bash interprets this literally and creates a file called `nul`, which is a
  reserved device name on NTFS and very hard to delete (needs admin/UAC). Use `2>/dev/null` instead.
- **Never use `rmdir /s /q`** — Bash's `rmdir` doesn't understand cmd.exe flags. Use `rm -rf` instead.
- **Never use `del`** — not available in Bash; use `rm`.
- **Never use `dir`** — use `ls`.
- For genuinely Windows-native commands, wrap them explicitly: `cmd /c "..."`.
- Default to Unix-style commands and paths in the Bash tool.
