# product_name

> RENAME: replace the title and every `product_name` reference in this repo
> (folder name, `pyproject.toml`, imports in `tests/`) before starting real
> development. See the checklist below.

One-line description of what this product does.

## Overview

A few sentences on what this product is, who owns it, and what problem it
solves. If this product is a **foundation product** (other products are
generated off it or depend on it), say so explicitly here and link to what
depends on it.

## Structure

```
├── src/product_name/
│   ├── __init__.py             # package init, version string
│   ├── main.py                 # entry point — config loading + run()
│   ├── example_module/         # rename/replace as real sub-modules are added
│   │   ├── __init__.py
│   │   └── core.py
│   └── business_rules/         # product-specific rules only — shared
│       ├── __init__.py           rules go in COMMON/j4lm-business-rules
│       └── variance_check.py     instead. Imported as a normal subpackage:
│                                  `from product_name.business_rules import ...`
├── tests/
│   ├── __init__.py
│   ├── test_main.py            # mirrors src/ — one test file per module
│   └── test_business_rules.py
├── docs/
│   ├── README.md                # scope of this folder — product-specific
│   │                             docs only, not shared guidance (that's
│   │                             COMMON/guidance)
│   ├── architecture.md          # components, data flow, dependencies,
│   │                             key decisions
│   └── data-dictionary.md       # field definitions — delete if this
│                                 product has no structured data to document
├── pyproject.toml               # package metadata, dependencies, tool
│                                 config (ruff, mypy, pytest)
├── .gitlab-ci.yml               # lint, type-check, test, build pipeline
├── .gitignore                   # standard Python/uv/editor exclusions
├── CODEOWNERS                   # required reviewers by path
└── README.md                    # this file
```

`business_rules/` lives inside the package (not at the repo root) so it can
be imported normally — a folder outside `src/` isn't part of the installed
package and can't be imported from code within it. `CODEOWNERS` still
targets it cleanly at its nested path (e.g.
`/src/product_name/business_rules/ @sme-username`).

Delete any of `docs/`, `business_rules/`, or `example_module/` that don't
end up being needed — don't leave them as empty placeholders.

## Business rules

If this product has product-specific business rules, they live in
`src/product_name/business_rules/` and are imported as a normal subpackage.
Delete that folder if none apply. Rules shared by 2+ products belong in
`COMMON/j4lm-business-rules` instead.

## Dependencies on shared packages

If this product consumes `COMMON/j4lm-pylib` or `COMMON/j4lm-business-rules`,
list the pinned version(s) here and in `pyproject.toml`. Never track `main`
for a shared dependency.

## Downstream / foundation product note

If this product generates output that other products consume (like SDDB),
document the release-tagging expectation here: downstream consumers should
pin to a specific release tag, never to `main`.

---

## Rename checklist (delete this section once done)

- [ ] Rename `src/product_name/` to the real package name (snake_case)
- [ ] Update `name` in `pyproject.toml` (kebab-case) and `packages` under
      `[tool.hatch.build.targets.wheel]`
- [ ] Update `--cov=src/product_name` in `pyproject.toml`'s pytest config
- [ ] Update imports in `tests/test_main.py` and `tests/test_business_rules.py`
- [ ] Replace the placeholder `main.py` logic with real functionality, or
      delete it if this product doesn't need a script entry point
- [ ] Rename/replace `example_module/` with real sub-modules, or delete it
      if `main.py` alone is sufficient
- [ ] Replace the example rule in `src/product_name/business_rules/` with
      real rules, or delete that folder if this product has no
      product-specific rules
- [ ] Fill in `docs/architecture.md` and `docs/data-dictionary.md`, or
      delete the `docs/` folder if not needed
- [ ] Fill in Overview, Usage, and ownership details above
- [ ] Update this repo's GitLab project Description field (Purpose / What
      goes here) per the convention in `COMMON/guidance`
