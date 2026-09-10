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

This is a living reference — sections get filled in and refined as I work through them. Practical exercises and scripts go alongside these notes as I write them, not just a link collection.

## Status

🟡 In progress — Phase 1
