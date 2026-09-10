# Bash Mastery — From Git Bash to Professional Linux CLI

A self-built path to real bash fluency — not just copy-pasting commands, but understanding what they do well enough to chain them, script them, and debug them. Built for bug bounty / security work, where recon, automation, and exploitation tooling all live in the terminal.

## Why this exists

Git Bash on Windows is a fine starting point but it's an emulation layer — it's missing real Unix internals (process management, cron, proper permissions, many core utils). Professional-level bash means being comfortable in an actual Linux environment. This course starts where you are (Git Bash) and deliberately pushes you toward a real Linux VM, because that's what real bug bounty / pentest / DevSecOps work runs on.

## Path

### Phase 1 — Git Bash Basics (Windows)
Get comfortable with the shell itself before worrying about "real" Linux.

- Navigation: `pwd`, `cd`, `ls`, `mkdir`, `rm`, `cp`, `mv`
- Viewing/editing files: `cat`, `less`, `head`, `tail`, `nano`
- Searching: `grep`, `find`
- Redirection & pipes: `>`, `>>`, `<`, `|`
- Permissions basics (limited on Windows, but learn the syntax): `chmod`, `chown`
- Git itself, since you're already using it daily: `git status`, `git add`, `git commit`, `git push/pull`, `git mv`, `git log`

**Goal:** stop looking anything up for everyday file/folder operations and git workflow.

### Phase 2 — Set Up a Real Linux Environment
This is the actual leap to professional-level tooling.

- Install a Linux VM (VirtualBox/VMware + Kali or Ubuntu) or use WSL2 as a bridge
- Get comfortable with a real package manager (`apt`), real permissions (`sudo`, `chmod` with actual effect), real process management (`ps`, `top`, `kill`, `jobs`, `&`, `fg`/`bg`)
- Learn the filesystem hierarchy (`/etc`, `/var`, `/usr`, `/home`) — professional tooling assumes you know this

### Phase 3 — Scripting
This is where bash stops being "commands you type" and becomes "programs you write."

- Variables, `$1`/`$2` positional args, `$@`, exit codes (`$?`)
- Conditionals: `if`/`elif`/`else`, `[[ ]]` test syntax
- Loops: `for`, `while`
- Functions
- Reading input: `read`, loops over file lines, loops over command output
- Quoting rules (`"$var"` vs `$var` — this trips everyone up eventually)

### Phase 4 — The Recon / Bug-Bounty Toolkit
Bash's real power for you specifically: gluing tools together.

- `curl` — making requests, headers, following redirects, sending data
- `jq` — since you already know this from your JSON course, combine it here (`curl ... | jq '.field'`)
- `grep -E`, `sed`, `awk` — text processing/extraction from tool output
- `xargs` — feeding output from one command as arguments to another
- Chaining recon tools: e.g. `subfinder | httpx | nuclei` style pipelines
- Writing your first recon automation script (loop over a list of subdomains, curl each, grep for something)

### Phase 5 — Debugging & Professional Habits
- `set -e`, `set -x` for debugging scripts
- Shellcheck (linting your scripts — professional teams expect clean bash)
- Writing scripts that fail loudly instead of silently
- Basic cron jobs for scheduled recon runs

## Notes


## Few hands-On Activities 

1. **The One-Liner Recon Chain**
   Using only `curl`, `grep`, and `sort`, hit `https://jsonplaceholder.typicode.com/users` and extract just the list of unique email domains (the part after `@`), sorted alphabetically — in a single piped command, no script file.

2. **Break Your Own Quoting**
   Create a variable containing a sentence with spaces, `$`, and a `*` in it. Try printing it four different ways: unquoted, single-quoted, double-quoted, and inside backticks. Predict the output before running each — then explain in your own words why each one behaved differently.

3. **Silent Failure Hunt**
   Write a 5-line script that deliberately does something wrong (e.g. reads a file that doesn't exist, or a command that fails) but *don't* use `set -e`. Watch it plow ahead silently. Then add `set -e` and `set -x` and watch the difference. Write down in one sentence why silent failures are dangerous in a recon script that's looping over 500 targets.

4. **Rebuild the Wheel: A Mini `httpx`**
   Given a text file of 10 URLs (mix real and fake ones), write a bash loop using `curl -s -o /dev/null -w "%{http_code}"` that checks each URL and prints only the ones returning `200`. No external tools — just `curl`, a `for` loop, and `if`.

5. **The `jq` + `bash` Combo Test**
   Fetch `https://jsonplaceholder.typicode.com/posts`, pipe it through `jq` to get only posts where `userId == 1`, extract just their `id` and `title`, then loop over each one in bash and `curl` its individual `/posts/{id}/comments` endpoint. This forces you to combine everything from your JSON course with bash looping.

6. **Permissions From Scratch**
   In your Linux VM (not Git Bash — this needs real permissions), create a script that only the owner can execute, and prove it fails for a different user. Then explain what `chmod 750` actually sets, digit by digit, without looking it up.

7. **Find the Needle**
   Given a directory of 50+ mixed files (some `.json`, some `.log`, some `.txt`), write one `find` + `grep` combo that finds every file modified in the last 7 days AND containing the word `token` — case-insensitive. No GUI search, no IDE — just the command line.

## Status

🟡 In progress — Phase 1
