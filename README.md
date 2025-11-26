# Malicious npm Package Scanner (`check.sh`)

**Zero false positives • Fast • No dependencies • Self-contained**

A battle-tested bash script that instantly scans your entire project (including monorepos) for **over 1,000 known malicious, hijacked, or compromised npm packages** that appeared in real-world supply-chain attacks during 2024–2025.

It **only alerts when a dangerous package is actually declared as a dependency** in any `package.json` — never triggers on mentions in READMEs, code comments, lockfiles, or documentation.

## Why this exists

In 2024–2025, npm saw an explosion of supply-chain attacks:
- Maintainer account compromises
- Typosquatting
- Protestware / malicious updates
- Scoped-package hijacks (`@posthog/*`, `@voiceflow/*`, `@ensdomains/*`, `@zapier/*`, etc.)

Many generic scanners generate noise. This script was built for **maximum signal, zero noise**.

## Features

- **Zero false positives** – only triggers on real dependencies (`"package-name": "version"`)
- Lightning-fast pure bash + grep (no Node.js needed)
- Fully self-contained – blocklist is embedded (no network calls)
- Recursively scans every `package.json` in the current directory and subdirectories
- Clear, color-free output perfect for CI/CD
- Works in monorepos, micro-frontends, or single-package projects

## Quick Start

```bash
# Download and run in one command
curl -fsSL https://raw.githubusercontent.com/your-repo/malicious-npm-scanner/main/check.sh | bash
```

Or:

```bash
# Save locally
wget https://raw.githubusercontent.com/your-repo/malicious-npm-scanner/main/check.sh
chmod +x check.sh
./check.sh
```

## Example Output

**All clear**
```text
Scanning for known malicious / hijacked packages...
============================================================
All clear! No known malicious or hijacked packages found in any package.json
```

**Danger found**
```text
DANGEROUS PACKAGE DETECTED → "@posthog/plugin-server"
 in → ./services/analytics/package.json:    "@posthog/plugin-server": "^1337.0.0"
 in → ./packages/telemetry/package.json:    "@posthog/plugin-server": "*"

DANGEROUS PACKAGE DETECTED → "jacob-zuma"
 in → ./package.json:    "jacob-zuma": "9.9.9"

WARNING: One or more dangerous packages were found. Remove them immediately.
```

## Use in CI/CD (GitHub Actions example)

```yaml
name: Security Scan

on: [push, pull_request]

jobs:
  scan-malicious-packages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Scan for malicious npm packages
        run: |
          curl -fsSL https://raw.githubusercontent.com/your-repo/malicious-npm-scanner/main/check.sh -o check.sh
          chmod +x check.sh
          ./check.sh
```

## Updating the blocklist

The blocklist is maintained and regularly updated.  
To get the latest version, just re-download the script.

## Contributing

Found a new malicious package that should be blocked?  
Open an issue or PR on the repository with evidence (NPM advisory, Snyk/Phylum/Socket report, etc.).

## License

MIT © 2025 Your Name / Community

**Stay safe out there.**
