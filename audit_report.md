# Dependency Constraint Audit — Python Service

Automated check of the service's pinned dependencies against the platform team's circulated
"still valid" version constraints. Verdicts were decided by a scripted checker that compares
versions as dot-separated integer tuples: a pin satisfies its claim only if it is **>= every
lower bound** and **< every upper bound**. Latest versions were looked up live on PyPI at
audit time; no value was guessed.

## Failing packages (3 of 7)

- **urllib3 == 1.26.18** — misses the lower bound `>=2.0.0`: 1.26.18 < 2.0.0, so the pin is a whole major version below the claimed floor.
- **numpy == 2.0.0** — misses the upper bound `<2.0.0`: 2.0.0 is not strictly less than 2.0.0; the pin sits exactly on the excluded boundary.
- **sqlalchemy == 2.0.30** — misses the upper bound `<2.0.0`: 2.0.30 >= 2.0.0, so the pin is already inside the range the claim excludes.

## Full results

| package    | pinned  | claimed constraint | verdict | latest version | latest-still-satisfies |
|------------|---------|--------------------|---------|----------------|-------------------------|
| requests   | 2.31.0  | >=2.28.0,<3.0.0    | PASS    | 2.34.2         | yes                     |
| urllib3    | 1.26.18 | >=2.0.0,<3.0.0     | FAIL    | 2.8.0          | yes                     |
| numpy      | 2.0.0   | >=1.23.0,<2.0.0    | FAIL    | 2.5.4          | no                      |
| pandas     | 2.2.2   | >=2.0.0,<3.0.0     | PASS    | 3.0.6          | no                      |
| flask      | 3.0.3   | >=2.2.5,<4.0.0     | PASS    | 3.1.3          | yes                     |
| sqlalchemy | 2.0.30  | >=1.4.0,<2.0.0     | FAIL    | 2.1.5          | no                      |
| click      | 8.1.7   | >=8.0.0,<9.0.0     | PASS    | 8.5.0          | yes                     |

## Notes for the platform review

- 3 of the 7 circulated claims are stale and are disproved by the current pins themselves
  (urllib3, numpy, sqlalchemy) — those pins can never have satisfied the claims as written.
- Even where a pin passes today, "just bump to latest" is not automatically safe under the
  claimed constraints: pandas 3.0.6 and sqlalchemy 2.1.5 (and numpy 2.5.4) would fall outside
  the claimed ranges, so the constraint set needs updating before any upgrade.
