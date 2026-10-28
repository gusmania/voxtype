# Security review — 2026-10-26/27

Updated the Rust lockfile within the existing dependency ranges and pinned patched Vite/esbuild versions for the documentation toolchain. The documentation dependency install reported zero known vulnerabilities and `npm run docs:build` passed.

This is a partial Rust remediation: OSV still flags glib 0.18, lru 0.12, block-buffer 0.10 and unmaintained transitive libraries. These require dependency migrations or upstream fixes. Do not interpret the lockfile update as a clean Rust audit. Full native validation details are in the workspace security report.

The branch excludes unrelated local prompt-enhancement changes and is published to the user's fork. No upstream pull request was opened. The local customized binary is built and deployed separately from this fork.

## Default-branch reconciliation — 27 October

Reconciled the security branch with the newer upstream-based `dev` branch (version 1.1.1), retaining its code and refreshing Cargo.lock against its current dependency ranges. The documentation production build passed and npm audit reported zero vulnerabilities. The refreshed OSV audit still reports block-buffer 0.10.4, glib 0.18.5, lru 0.12.5 and maintenance notices for paste, proc-macro-error, zbus_names, zvariant and zvariant_derive.

Validation of the reconciled tree: `cargo check --locked --no-default-features` passed. This does not validate every optional inference backend or establish a clean Rust security audit.

## ONNX Runtime compatibility

The full Parakeet build exposed an API incompatibility when an unconstrained prerelease range selected ONNX Runtime rc.15. Pin `ort` to `=2.0.0-rc.12`, the API expected by the current engine implementations; the resolver selects compatible Parakeet 0.3.6 and ort-sys rc.12. The resulting lockfile has the same remaining OSV findings listed above.

The compatibility fix passed `cargo check --locked --features parakeet`, explicitly validating the engine omitted from the earlier default-feature check.
