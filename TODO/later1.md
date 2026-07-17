# j4lm-pylib

Shared, versioned Python utility library used across TC4JLM products.

## Overview

Reusable functions and modules with no product-specific logic — general
utilities that 2 or more products genuinely use. This is **not** the place
for business rules (those go in `COMMON/j4lm-business-rules` or a product's
own `business_rules/`) or anything specific to a single product's data or
process — see `where-does-this-go.md` in `COMMON/guidance` for the
shared-vs-specific test.

## Structure

```
├── src/j4lm_pylib/
│   ├── __init__.py           # package init, version string
│   └── <module>/              # one subpackage per functional area
│       ├── __init__.py
│       └── ...
├── tests/                    # mirrors src/ — one test file per module
├── docs/                     # architecture/design notes specific to this
│                               library, if needed beyond this README
├── pyproject.toml            # package metadata, dependencies, tool config
├── .gitlab-ci.yml            # lint, type-check, test, build, publish
├── .gitignore
├── CODEOWNERS
└── README.md                 # this file
```

## Consuming this library

Install a **pinned version** from the GitLab Package Registry:

```
j4lm-pylib==1.2.0
```

in the consuming product's `pyproject.toml`. Never track `main` or install
unpinned — a change here that breaks a product should surface as "the
pinned version needs bumping," not as an untraceable break the next time
someone reinstalls dependencies.

## Adding to this library

Add a function here only once **2 or more products actually use it** —
not speculatively "in case something needs it later." Promoting code here
preemptively is how shared libraries turn into untrustworthy junk drawers
that nobody fully trusts or understands. If you're the first person who
might need something like this, it likely belongs in your product's own
code until a second, real use case shows up.

## Versioning and releases

This library is released with semantic versioning (semver) tags:

- **Patch** (`1.2.0` → `1.2.1`): bug fix, no behavior change for existing callers
- **Minor** (`1.2.0` → `1.3.0`): new functionality, backward-compatible
- **Major** (`1.2.0` → `2.0.0`): breaking change to an existing function's
  signature or behavior

Every release should have a corresponding entry in `CHANGELOG.md` (add one
if it doesn't exist yet) so a product owner deciding whether to bump their
pinned version can tell what changed without reading the diff.

## Review

Changes here affect every product that depends on this library — see
`CODEOWNERS` for who needs to review before a merge request here is
approved.
