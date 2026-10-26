## Lab 3 — Backup files — notes
robots.txt listed /backup, leading to leaked source (.java file) containing a
hardcoded DB password. Sub-pattern of info disclosure: leftover file, not a
diagnostic tool (compare lab 2's phpinfo.php, which the app actively runs and
serves).

Read the whole file, not just for the "answer" — real leaked code often has
multiple issues stacked together (this one had three: the password, a SQL
injection pattern via string concatenation, and a Java deserialization gadget
shape — only the password was this lab's objective).

**How to spot the actual secret among normal-looking arguments:** look for the
value that doesn't match the pattern of the others. Mundane, human-typed
values (hostnames, default usernames, standard ports) sit next to one
genuinely random, high-entropy string — that contrast IS the signal. Also
recognize common builder-method shapes (JDBC connection args follow a
near-universal driver/type/host/port/db/user/password order) — once the
pattern's recognized, position confirms what you already suspect from entropy
alone.

**Reachability vs. content:** finding the artifact is step one, not the
finding. A backup file or debug page with nothing sensitive inside is still
real (unnecessary exposure) but weak on its own — severity comes from what's
actually inside: credentials, CVE-linked versions, internal architecture.
