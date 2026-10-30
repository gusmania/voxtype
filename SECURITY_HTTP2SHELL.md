# HTTP2Shell runtime remediation — 2026-10-30

CVE-2026-111497 (HTTP2Shell / Node2Shell / Broccoli2Shell) affects the native
Brotli decoder used by Node.js. An npm dependency audit alone cannot detect or
fix the bundled runtime. Exposure requires decompressing attacker-controlled
Brotli input, including HTTP responses; no exploit was run against this project.

Use Node.js 22.23.4 or a newer patch/minor in the same major line (`nvm install`).
The package engine requirement and `.npmrc` reject older runtimes during npm
installation. Docker and CI runtime selectors are pinned where present.
These settings do not upgrade an already running process.

Rebuild Docker deployments with `docker compose build --pull`, then recreate the
application containers. Verify `node -p "JSON.stringify(process.versions)"` inside
each running Node container. For managed hosting, redeploy and verify both build
and function runtimes; providers control the exact function patch version.
Do not assume a successful Git push means the running service is patched.

Static browser bundles do not contain the vulnerable Node runtime; their build
and development tools still need a patched runtime. Native desktop Rust code is
not remediated by this Node configuration change.

Source: https://nodejs.org/en/blog/vulnerability/october-2026-security-releases

Documentation CI also pins Bun 1.4.4, which includes the Brotli fix.
