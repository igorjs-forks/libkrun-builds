# Contributing to libkrun-builds

This project follows the standard igorjs contribution rules. Start here:

- **[.github/CONTRIBUTING-RULES.md](.github/CONTRIBUTING-RULES.md)**: DCO, CLA, commit conventions, PR process, and the code style baseline (SPDX headers, dependency policy, commit signing). These rules are shared across all igorjs repos.
- **[README](README.md)**: project description, build instructions, test commands, and tooling setup.
- **[SECURITY.md](SECURITY.md)**: vulnerability disclosure process.

## Project-Specific Notes

This repo is a vendor pipeline: it builds upstream libkrun + libkrunfw and publishes relocatable tarballs as GitHub Releases. Contributions typically fall into:

- **Build script fixes** (`build.sh`): cross-platform issues, missing dependencies, install-name relocations.
- **CI workflow improvements** (`.github/workflows/release.yml`, `watch-upstream.yml`): matrix expansion, signing pipeline, attestation flow.
- **Documentation** (README, SECURITY.md): consumer flow, verification, troubleshooting.

Bugs in the upstream libraries themselves should go to [containers/libkrun](https://github.com/containers/libkrun) or [containers/libkrunfw](https://github.com/containers/libkrunfw), not here.


## Project-Specific Code Style

Beyond the baseline rules:

- **Bash scripts** use `set -euo pipefail` at the top, with `IFS=$'\n\t'` for safer field-splitting where applicable.
- **YAML workflows** pin all third-party actions to commit SHAs (see Dependabot config for automatic refresh).
- **No new runtime dependencies** in `build.sh` beyond what's listed under "Required tools" in the README.


## Project-Specific Tests

There's no automated test suite for the build itself; verification happens via:

- `actionlint .github/workflows/*.yml` for workflow validity.
- `bash -n build.sh` for bash syntax.
- The release workflow's matrix build is the integration test; check the run logs for any non-zero exits.

For changes to `build.sh`, run it locally for at least your host's target triple before opening a PR. For workflow changes, trigger a workflow_dispatch run against the PR branch before merging.

