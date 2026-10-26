# Information Disclosure — Research Notes

## 5 Lab Patterns

### 1. Error Messages
Trigger an error (invalid input, wrong type) → read stack trace
Signal: framework version, file path, server info
Real world: version → google CVE → instant escalation

### 2. Debug Page
Browse to /cgi-bin/phpinfo.php or similar debug endpoints
Signal: full server config, environment variables, secret keys
Real world: check robots.txt for hidden paths, fuzz common debug URLs

### 3. Backup Files
Fuzz known files with backup extensions: .bak .old .orig ~ .swp
e.g. /index.php.bak → source code exposed
Real world: every file you find, try appending backup extensions

### 4. HTTP Headers / Auth Bypass
Read response headers → X-Powered-By, Server, custom headers
Signal: tech stack, internal IPs, admin methods (TRACE, CUSTOM)
Real world: send OPTIONS/TRACE requests, read every response header

### 5. Version Control History (.git exposed)
/.git/ accessible → use GitTools Dumper + Extractor
Commands:
  bash gitdumper.sh https://TARGET/.git/ ~/git-dump
  bash extractor.sh ~/git-dump ~/git-extracted
  cat git-extracted/*/admin.conf  (or any config file)
Signal: deleted passwords still in commit history
Real world: always check /.git/ on every target
