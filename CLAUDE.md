# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of standalone DXL scripts for IBM Rational DOORS 9.5. Each `.dxl` file is an independent program; there are no shared includes between them, no build, no linter and no test suite. Scripts only run inside DOORS, so they cannot be executed or verified from this environment. Say so when reporting a change rather than claiming it was tested.

`README.md` documents every script for users. Keep it in sync when adding, renaming or changing the behavior of a script (overview table plus a section per script).

## Running scripts

- Install: copy the repo contents into `C:\Program Files (x86)\IBM\Rational\DOORS\9.5\lib\dxl\addins\user`. The folder is a DOORS addins library, so `user.hlp` and `user.idx` must stay at the top level.
- Interactive: from a module window, use the **User** menu (for scripts listed in `user.idx`), or **Tools → Edit DXL...** to load and run any file.
- Batch (no GUI), as used for the scheduled HTML export:
  `"C:\Program Files (x86)\IBM\Rational\DOORS\9.5\bin\doors.exe" -u "USERNAME" -P "PASSWORD" -p "PROJECT_NAME" -b "addins\user\ExportAllModulesIntoHTML.dxl" -W`

## Addins library structure

- `user.hlp`: library description (first line is the one-line description). Required by DOORS.
- `user.idx`: User menu entries, one per line: `<file name without .dxl> <mnemonic><accelerator> <menu label>`. In this repo the second field is two characters: `_` means none, so `_4` is Ctrl+4 and `__` is no shortcut. A line of only hyphens inserts a separator. Scripts not listed here are run manually from the DXL editor.
- Every script must start with the header DOORS expects: line 1 is a `//` one-line description (shown in the DXL Library window), followed immediately by a `/* ... */` block with the longer description (shown by its Describe button). The repo convention adds an `Author:` line inside that block and a history block:

  ```
  //**********************************  History  ******************************
  // M. Serdar Karaman  YYYY-MM-DD  Description of change
  ```

  Some scripts also have a `TODO` block after the history. Append a history line when modifying a script.

## DXL conventions used here

- `pragma runLim, 0` disables the DXL execution timeout; use it in any script that loops over whole modules or folders.
- Scripts operate on `current` (module or folder), so they depend on where they are launched. Folder-wide scripts recurse over the `Item`s of `current Folder`, skipping deleted items: `ListDerivedReqs.dxl` and `ListSafetyReqs.dxl` with `scanFolder` (folders only), `ExportAllModulesIntoHTML.dxl` with `collectModules` (folders and sub-projects), which gathers the module list first so a progress bar can be shown.
- Skip lists made with `create` compare string keys by address, so their iteration order is not insertion or alphabetical order. Use `createString` for string keys and never rely on a skip list's order for something ordered like path segments.
- Long-running scripts must not open modal dialogs (`ack`, `errorBox`, `confirm`, `query`): they block batch runs and can hide behind the main window. `ExportAllModulesIntoHTML.dxl` prints `ERROR:` lines instead, shows a progress bar only when `!isBatch()`, and keeps opened modules under control with `utils\openModulesMemoryManagement.inc`.
- Linked modules are opened read-only with `read(fullName(...), false)` before following links; scripts that modify data use `edit(...)`.
- DXL parses juxtaposition right-associatively, so a one-argument function call followed by a concatenation can swallow it: `getenv("USERPROFILE") "\\Desktop"` is parsed as `getenv("USERPROFILE\\Desktop")` and returns null. It compiles without a warning when the argument type still matches. Put the call in its own statement, or wrap it in parentheses, before concatenating.
- `mkdir` and other perms halt the script on failure. Wrap them in `noError()` ... `lastError()` to log the error and continue.
- Attribute values are coerced to strings with `o."Attr" ""`. Several scripts rely on the author's organization's attribute schema: `Req` (`N`, `D`, empty), `Safety Requirement` (`True`/`False` or `Yes`/`No`), `Safety Related`, `Means of Verification`, and on module names containing `HLR_`, `PIDS` or `ICD`. Keep these names unless asked to change them.
- Scripts without a selection show a small modal `DB` dialog ("No object is selected!") instead of failing silently; follow the same pattern for selection-based scripts.
- File outputs go under `%USERPROFILE%`: `.DoorsExport` (temporary baseline exports for `CompareBaselines.dxl`, which launches Beyond Compare 4/5 from its default install path) and `Desktop\DoorsExport` (HTML report).
- `delete(Object)` is a permanent (hard) delete in DXL, not a soft delete. `PurgeDeletedObjects.dxl` keeps its `edit(...)` line commented out on purpose to prevent accidental runs; do not re-enable it.

## DXL reference

`docs/` holds the DXL reference manual (`dxl_reference_manual.pdf`, `dxl.chm`) and `AllCommands.dxl`, a list of every DXL function signature. Grep `AllCommands.dxl` first to confirm a function exists. For its semantics, extract the manual to text with `pdftotext -layout docs/dxl_reference_manual.pdf <scratch>/dxl.txt` (available in Git Bash) and grep it. Do not guess DXL APIs from other languages.

## Files and encoding

Files use CRLF line endings (`core.autocrlf=true`). Most scripts are ASCII; some are UTF-8 because of Turkish author names or non-ASCII literals. Keep each file's encoding when editing. In `ExportToJSON.dxl`, the umlaut character literals in `urlEscape` have already been lost to U+FFFD replacement characters.
