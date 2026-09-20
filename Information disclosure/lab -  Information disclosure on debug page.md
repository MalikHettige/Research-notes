## Lab 2 — Debug page — notes

**What's the actual bug/technique?**

The app left a debug diagnostic tool (phpinfo.php — a built-in PHP function that dumps full server/environment config) reachable with no access control, in a production environment. Technique = recognizing common per-framework debug tooling and reading everything it reveals, not guessing randomly.

**How do I know an artifact is "the one"? Just by trying?**

Not blind trying — pattern-matching. Debug endpoints aren't infinite or random; they're a small, well-known set per tech stack:
  - PHP: /cgi-bin/phpinfo.php, /phpinfo.php, /info.php
  - Spring Boot: /actuator/env, /actuator/heapdump, /actuator/beans
  - Go: /debug/pprof
  - Symfony: /_profiler
  - ASP.NET (classic): /elmah.axd
  - Laravel: /telescope, /_ignition

Fingerprint the stack first (response headers, error messages, cookie naming conventions), THEN test only the debug paths relevant to that stack — not every path against every target.

In labs, confirmation is built in — the objective states exactly what value you're hunting, and it's a labeled field (SECRET_KEY) sitting in plain sight. In real programs there's no such statement. "The one" is confirmed by whether it changes what you can actually do — a version tied to a known CVE, a credential you can authenticate with, an internal hostname that expands your attack surface. A page that just says "debug: on" with nothing usable inside isn't nothing, but it's a much weaker finding than one with real data in it.

**How do I build a list of these endpoints?**
- SecLists (github.com/danielmiessler/SecLists) — curated wordlists for exactly this, under Discovery/Web-Content and tech-specific lists
- Every time you personally find a new one, tag it to the framework it
  belongs to and add it to a running list in Research-notes — same repo you're already using, just make the habit deliberate instead of ad hoc
- Once the stack is known, run ffuf/feroxbuster against the matching list instead of testing paths one at a time by hand

**Is the leaked secret itself the bug?**
No — same Root Cause vs. Impact split you already use in every writeup:
  - Root cause = the debug page being reachable at all with no access control
  - SECRET_KEY = the impact, the proof it's not just theoretical

A debug page with nothing sensitive inside is still a real, lower-severity finding on its own — unnecessary exposure of internal config/versions. Finding an actual secret is what escalates it, because of what that secret then enables. Concrete example worth remembering: frameworks like Flask and Django sign session cookies using exactly this kind of SECRET_KEY — leaking it can mean forging arbitrary sessions, including an admin one. That's why this specific leak is treated as high-value rather than just "info disclosed."
