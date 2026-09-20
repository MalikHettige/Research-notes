## Lab 4 — Authentication bypass via information disclosure — notes
TRACE on /admin leaked X-Custom-IP-Authorization, auto-appended by a reverse proxy, holding the caller's real IP. App trusts this client-supplied header at face value to decide "local user" access — set it to 127.0.0.1 on a normal GET request, admin panel opens.

Pattern: same family as lab 3's hardcoded secret in leaked source, but the disclosed thing here is a *mechanism* (a header name and how it's trusted), not a credential. Confirms the earlier answer to "does the bug always need a secret inside" — no; leaking *how the check works* can be the bypass itself.

**Mistake caught mid-attempt:** sent TRACE with the header added, got rejected
— TRACE only echoes the request back, it never runs real app logic. Header changes only matter on the actual method the app processes normally (GET).

**What TRACE actually is:** a diagnostic method that echoes back the fully processed request, after any proxies/gateways in front of the app have already modified it — that's why it can reveal headers you never sent.

**Real-world caveats:**
- Header names in the wild won't literally be "X-Custom-IP-Authorization" — either find it disclosed (TRACE, leaked error, leaked source) or check known conventions: X-Forwarded-For, X-Real-IP, X-Client-IP, True-Client-IP, CF-Connecting-IP. Param Miner automates this guessing at scale.
- TRACE is frequently disabled on hardened production servers (historically due to Cross-Site Tracing/XST) — don't rely on it as the only technique.
- Never execute a destructive action (delete, modify) against a real user's data on a live program without explicit authorization — prove impact against your own account, or describe the risk without executing it.
