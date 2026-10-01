# Security Policy

## Scope

This repository contains standalone DXL scripts that run inside IBM Rational DOORS 9.5. Relevant issues include scripts that:

- modify or delete data unexpectedly (for example `delete(Object)` is a permanent delete)
- write files outside the documented locations under `%USERPROFILE%`
- execute external programs or commands in an unsafe way
- expose credentials, for example in batch command lines or logs

## Supported versions

Only the latest version on the `main` branch is supported.

## Reporting a vulnerability

Please do not open a public issue for security problems. Use GitHub's private reporting instead:
[Report a vulnerability](https://github.com/Mavrikant/DOORS-DXL-Scripts/security/advisories/new).

Include the script name, DOORS version, steps to reproduce and the impact. You can expect an initial response within a few days.

## Safe use

Scripts here are provided as is. Review a script and try it on a test project or a copy of your data before running it on production modules.
