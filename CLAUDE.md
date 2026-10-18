# ProsperoStore

## Git

- Commit and push straight to `main`. No branches or pull requests.
- Never add Claude as a collaborator: no `Co-Authored-By` trailer, no "Generated with Claude" line, in commits, tags, or PR text.

## Lint

The CI runs `LINT_SKIP_TIDY=1 make lint` (`tools/lint.sh`). Run `LINT_SKIP_TIDY=1 bash tools/lint.sh` before every commit; it must exit 0. A failed lint also fails the build that publishes a release.

- **Formatting:** clang-format 18 with the repo's `.clang-format`, over `src/` (except `src/third_party`), `tooling/native`, `tests/` and `examples/`. Run `bash tools/run_clang_format.sh` to fix, `--check` to verify. Do it after every edit, including the lines of existing files you touch.
- **Header on every source file:** the first 20 lines of each `.c .cc .cpp .h .hpp .ld .py .ps1 .sh .yml .yaml`, `Makefile`, `.clang-format`, `.clang-tidy` and `.env.example` must contain both `Copyright (C) 2026 BlackBearReloaded` and `SPDX-License-Identifier: GPL-3.0-or-later`. New files need them. `third_party/` and `src/third_party/` are exempt.
- **No C# files:** `.cs` and `.csproj` fail the lint.
- **`param.json`:** `titleId` is `PPSA` plus five digits; `contentVersion` is `NN.NNN.NNN`; `masterVersion` is `NN.NN`; `downloadDataSize` is a non-negative integer; `titleName` of the default language is not empty.
- **No local paths or personal names:** `C:\Users\`, `/home/denis`, `/mnt/c/Users/denis` and `Denis` anywhere in tracked files fail the lint.
- **Whitespace:** `git diff --check` must be clean (no trailing whitespace).
- **Shell scripts:** every `tools/*.sh` must pass `bash -n`. `tools/validate-assets.sh` also runs.
- **Static analysis:** `tools/run_clang_tidy.sh` runs only without `LINT_SKIP_TIDY=1`. The CI skips it.

## Version

A release tag is `vN.NNN.NNN` and must match `contentVersion` in `sce_sys/param.json` (`01.000.100` is tag `v1.000.100`); the workflow refuses a mismatch. A version bump changes the same five files as the `Version 01.000.090` commit: `sce_sys/param.json`, `src/platform/ps5/store_http.cpp`, `src/diag/diagnostics.cpp`, `src/net/curl_request.cpp` and `.github/ISSUE_TEMPLATE/bug_report.yml`. Tags here are lightweight.
