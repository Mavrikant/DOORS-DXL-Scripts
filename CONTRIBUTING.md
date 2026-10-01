# Contributing

Thanks for helping improve these DOORS DXL scripts. Please follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Reporting bugs and requesting features

Use the issue templates. Include your DOORS version, the script name, how you launched it (User menu, DXL editor or batch) and the DXL error output if any.

## Pull requests

1. Fork the repository and create a branch from `main`.
2. Keep each script standalone; there are no shared includes between scripts.
3. Start every script with the header DOORS expects: line 1 is a `//` one-line description, followed immediately by a `/* ... */` block with the longer description and an `Author:` line, then a history block:

   ```
   //**********************************  History  ******************************
   // Your Name  YYYY-MM-DD  Description of change
   ```

4. Append a history line when you modify an existing script.
5. If a script should appear in the **User** menu, add it to `user.idx`. Keep `user.hlp` and `user.idx` at the top level.
6. Update `README.md` (overview table and the script's section) when you add, rename or change a script's behavior.
7. Long-running scripts should use `pragma runLim, 0` and avoid modal dialogs so they work in batch mode.
8. Keep each file's encoding and use CRLF line endings.

## Testing

There is no automated test suite; scripts only run inside DOORS. Describe in the pull request how you tested the change (DOORS version, test module) or state that you could not.
