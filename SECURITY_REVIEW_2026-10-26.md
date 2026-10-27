# Security review — 2026-10-26/27

Updated the Rust lockfile within the existing dependency ranges and pinned patched Vite/esbuild versions for the documentation toolchain. The documentation dependency install reported zero known vulnerabilities and `npm run docs:build` passed.

This is a partial Rust remediation: OSV still flags glib 0.18, lru 0.12, block-buffer 0.10 and unmaintained transitive libraries. These require dependency migrations or upstream fixes. Do not interpret the lockfile update as a clean Rust audit. Full native validation details are in the workspace security report.

The branch excludes unrelated local prompt-enhancement changes and is published to the user's fork. No upstream pull request or production deployment was performed.
