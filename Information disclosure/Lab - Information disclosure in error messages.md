# Lab: Information disclosure in error messages

## Core principle
Read everything the app outputs, especially what you'd normally skim past —
errors, 404s, comments, headers. The leak isn't hidden; it's ignored.

## Where to look
**Force errors:** break expected types on every param (string→number, huge
values, negative, empty, `'` `"` `<` `../`). Read the FULL body on any 500.

**Forgotten by devs:** page source + JS comments, robots.txt, debug endpoints
(`/debug`, `/status`, `/actuator`, `/phpinfo.php`, `/.env`)

**Leftover files:** `.bak` `~` `.old` `.swp`, `.DS_Store`, exposed `/.git/HEAD`

**Server-told-you-anyway:** response headers (`Server`, `X-Powered-By`),
`TRACE` requests, error-message wording differences

## Is it actually sensitive?
- Does it skip a step an attacker would otherwise need? (version → known CVE)
- Does it narrow the target? (file paths, framework names, hostnames)
- Is it directly usable alone? (creds, keys, secrets)

If none apply → informational, don't chase it as the headline finding.

## Quick reference

| Leak type | Signal | Enables |
|---|---|---|
| Stack trace | Framework + version | CVE lookup → exploit chain |
| Debug page | Env vars, config dump | Secrets, architecture |
| Backup file | Old source code | Hardcoded creds, logic exposure |
| `.git` exposure | Commit history | Secrets removed later, still in history |
| Verbose headers | Server/framework banner | Same as stack trace |
