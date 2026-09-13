# Emergent Repository Intelligence (stuff.md)

> Cross-commit patterns, architectural hotspots, and Gemini-powered insights.

## Deterministic patterns

**Most-touched files (Architectural hotspots):**
- `watchman.ts` — 175 commits
- `build/deps/github_hashes/facebook/fb303-rev.txt` — 11 commits
- `build/deps/github_hashes/facebook/fbthrift-rev.txt` — 11 commits
- `build/deps/github_hashes/facebook/mvfst-rev.txt` — 10 commits
- `build/deps/github_hashes/facebook/wangle-rev.txt` — 10 commits

**Files with iterative wrong->correct cycles (Hard-won lessons):**
- `watchman.ts` — 12 failed-and-fixed cycles

**Recurring themes in commit subjects:**
- `updating` — 94 commits
- `hashes` — 94 commits
- `summary` — 25 commits
- `type` — 12 commits
- `build` — 11 commits
- `getdeps` — 10 commits

--- 

## LLM-surfaced patterns (Gemini)

As the Commit Archaeology Engine (CAE), I have analyzed the provided digest. My findings regarding the repository's trajectory and maintenance patterns are as follows:

### Architectural Hotspots and Failure Classes
*   **Hash/Dependency Fragility:** A significant cluster of commits (marked `WRONG`) involves "Updating hashes" followed immediately by a corrective commit. This pattern indicates that the process for generating or updating dependency hashes is error-prone, likely due to sensitivity to environmental factors (e.g., specific CMake versions, host OS configurations) not captured by the automated update script.
*   **CI/CD Configuration Drift:** Failures are frequently associated with `watchman.ts` configuration changes. Recurring issues involve compiler version pinning (GCC, LLVM), timeout settings for CI runners, and build toolchain inconsistencies (e.g., `sccache`, `delocate`).
*   **Abstraction Leaks:** Evidence of ongoing architectural tension between internal tooling and Open Source (OSS) build requirements. Developers frequently modify `watchman.ts` to accommodate OSS-buildable library consumption or to strip internal-only metadata (`BUCK`, `PACKAGE`) to maintain a viable public-facing build.

### Skill Growth and Evolution
*   **Infrastructure Formalization:** There is a clear transition from ad-hoc dependency management toward standardized manifest-based builds. The introduction of `build/fbcode_builder/manifests` and the migration of legacy build logic to structured CI workflows (Sandcastle, GitHub Actions) demonstrate a move toward more reproducible engineering environments.
*   **Telemetry and Observability Maturity:** The recent commits (late August to September 2026) show an increased focus on structured logging (e.g., `WatchmanXplatLogger`, structured error schemas). This represents a shift from legacy error handling to a more diagnostic-forward architecture.
*   **Standardization of Types:** The refactor in `eden.thrift` (extracting `GlobPath` typedefs) indicates an evolution toward cleaner interface definitions, reducing reliance on container-level annotations in favor of explicit C++ type safety.

### Key Architectural Decisions and Hard-Won Lessons
*   **Fallback Complexity:** The modification of `FSDetect.cpp` to distinguish between "edenfs:" sources and generic NFS mounts highlights a fundamental challenge in managing distributed filesystem identities. The lesson learned is that filesystem-type detection is inherently ambiguous and requires centralized logic to prevent misclassification.
*   **OSS Compatibility as a Constraint:** The prevalence of `watchman.ts` modifications to ensure OSS buildability reveals that maintaining an open-source mirror is a primary architectural driver, forcing the team to solve dependency resolution issues (e.g., `c-ares`, `rocksdb` pathing) that would otherwise be trivial in a monolith environment.
*   **Deployment Safety:** The decision to keep the "master enable-error-logging gate" while deprecating old config keys during the `eden.thrift` rollout indicates a disciplined approach to managing breaking changes in complex production systems, prioritizing compatibility shims to ensure smooth transitions.