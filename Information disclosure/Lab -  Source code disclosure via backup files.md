## Lab 3 — Backup files — notes
robots.txt listed /backup, leading to leaked source (.java file) containing a
hardcoded DB password. Same principle as lab 1: the app hands you information
it shouldn't, you just have to be looking. Leaked source code is a jackpot
category — read the whole file, not just for the "answer," since real code
often contains multiple separate issues stacked in the same leak (this one had
three: the password, a SQL injection pattern, and a deserialization gadget
shape — only one was this lab's actual objective).
