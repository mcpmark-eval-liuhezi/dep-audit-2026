# Audit Plan

## Scope
We start from the starred-repositories shortlist (OpenBMB/AgentCPM, PaddlePaddle/ERNIE, FudanNLP/fastNLP) and work through each one as a tracked child issue under the master tracker.

## Steps
1. Inventory dependencies for each project (requirements, setup, lockfiles, environment files).
2. Compare pinned versions against current upstream releases to spot stale or abandoned packages.
3. Check each dependency for known security advisories and unmaintained versions.
4. Note license compatibility and any transitive dependencies pulled in by heavy packages.
5. Record findings in the matching audit issue and file follow-up work there.

## Outcome
A prioritized list of upgrades and replacements, with the riskiest dependencies flagged first, ready to act on once the audit is complete.
