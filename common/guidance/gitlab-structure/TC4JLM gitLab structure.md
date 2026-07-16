# TC4JLM GitLab Structure — Complete Listing

This is the full current listing of every group and project under `TC4JLM`, with a
detailed description of each. For help deciding where *new* work should go, see
`where-does-this-go.md` — this document is a reference, not a decision guide.

**Legend:** **Group** = organizational container only, holds other groups/projects.
**Project** = an actual Git repository (code, docs, commit history).

---

## Quick and dirty: where does this go?

| If your work is... | Put it in |
|---|---|
| A personal experiment, nobody else depends on it | `playground/<your-username>` |
| Reusable docs, code, business rules, CI config, or a project scaffold used by 2+ products | `common/` (see which specific repo below) |
| Team-level, multiple contributors, not yet owned like a product | `labs/<name>` |
| An owned product with a real owner and users | `products/<name>` |
| Something other products are generated off of / depend on (e.g. SDDB) | `products/<name>`, flagged as a **foundation product** |
| A one-off response to a specific data request (RFI) | `rfis/rfi-<year>-<sequence>-<short-slug>` |
| Internal training/enablement material (e.g. the Git/GitLab class) | `common/<training-project-name>` — flat, no subgroup, unless multiple classes accumulate |
| A live workspace for one training cohort's exercises | `labs/<training-project-name>-<year>-<cohort-id>` |
| Pre-migration backup (Advana requirement) | `archive/` |
| Genuinely doesn't fit any row above | Don't create it yourself — see Exceptions in `where-does-this-go.md` |

---

## `archive` (Group)

**Purpose:** Advana-required backup of pre-migration code. Read-only — not for active development.
**What goes here:** One or more backup projects containing pre-migration snapshots.

### Projects under `archive`

*(one or more backup projects — no active naming convention yet, since nothing new is created here)*
- **Purpose:** Preserve a snapshot of code as it existed before the GitLab reorganization.
- **What goes here:** Whatever existed at time of backup. No new commits expected.

---

## `common` (Group)

**Purpose:** Shared resources used across all products — docs, the shared Python library, shared business rules, CI templates, the project scaffold, and internal training material.
**What goes here:** The six projects listed below. Nothing is developed directly in `common` itself — only inside its projects.

### `common/guidance` (Project)

- **Purpose:** Policy, standards, and onboarding docs for TC4JLM.
- **What goes here:** Markdown docs organized into folders (`policies/`, `standards/`, `onboarding/`), with a root index README. Includes `where-does-this-go.md` and this document.

### `common/j4lm-pylib` (Project)

- **Purpose:** Shared, versioned Python utility library.
- **What goes here:** Reusable functions/modules used across products. `src/` layout, published to the GitLab Package Registry. Consumed via a pinned version — never vendored or copy-pasted into a product repo.

### `common/j4lm-business-rules` (Project)

- **Purpose:** Business rules shared by 2+ products.
- **What goes here:** Versioned rule modules, SME-reviewed, semver-tagged. Changes require sign-off — see CODEOWNERS. Rules that only apply to one product stay in that product's own `business_rules/` folder instead.

### `common/j4lm-ci-templates` (Project)

- **Purpose:** Shared CI/CD pipeline definitions.
- **What goes here:** Reusable `.gitlab-ci.yml` includes covering lint, type-check, test, build, and deploy stages. Product repos include these rather than duplicating pipeline stages locally.

### `common/j4lm-project-template` (Project)

- **Purpose:** Starter scaffold for new product repos.
- **What goes here:** Starter `src/` layout, `pyproject.toml`, CI config, and README template. Used when creating any new project under `products/`.

### `common/gitlab-training` (Project)

- **Purpose:** Git/GitLab curriculum for internal training, taught in the Advana Databricks environment.
- **What goes here:** Course material and exercise scaffolding. No `j4lm-` prefix (never referenced outside its GitLab path). Stays flat under `common/` rather than nested under a `training/` subgroup unless a second/third class makes a shared parent worthwhile. The *curriculum* lives here; the *live per-cohort exercise workspace* lives in `labs/` (see below).

---

## `products` (Group)

**Purpose:** Owned, production-bound work with real users and delivery targets.
**What goes here:** One project per product, listed below.

### `products/sddb` (Project — Foundation product)

- **Purpose:** Core product that other products are generated off of. Maintained by its own dedicated team.
- **What goes here:** Core codebase, `bteq/` (legacy Teradata load scripts, flagged as deprecating), `business_rules/` (sddb-specific rules), release-tagged (semver). Downstream consumers pin to release tags, never to `main`. Does **not** include validation or downstream reporting — those are separate repos maintained by different teams (see below).

### `products/sddb-validation` (Project)

- **Purpose:** Validates each monthly sddb data pull and produces the go/no-go signal analysts check before using that month's data. Maintained by a separate team from sddb's core codebase.
- **What goes here:** Schema checks (row counts, null rates, expected columns/types), business-rule checks (thresholds/ranges tied to sddb's data), and a report module producing the pass/fail signal. Pinned to the sddb release tag it validates. Runs on a monthly schedule.

### `products/sddb-sankey-report` (Project)

- **Purpose:** Quarterly Sankey visualization built from validated sddb output, for analyst use. Maintained by a separate team from sddb's core codebase.
- **What goes here:** Extract/transform/render modules pulling from sddb's validated release. Pinned to a specific sddb release tag. README notes which sddb version and validation pass it depends on. Runs on a quarterly schedule.

### `products/<name>` (Project — template for future products)

- **Purpose:** Owned, production-bound product with real users.
- **What goes here:** Standard `src/`, `tests/`, `pyproject.toml`, `business_rules/` (if any product-specific rules exist), CI config. Built from `common/j4lm-project-template`. See `common/guidance` for repo standards.

---

## `labs` (Group)

**Purpose:** Team-level, pre-product work — multiple contributors, not yet owned like a product.
**What goes here:** One project per initiative.

### `labs/<name>` (Project — template)

- **Purpose:** Incubating work with an uncertain future — not yet known whether it becomes a real product.
- **What goes here:** Whatever the initiative needs. Graduates to `products/` once it has an owner, real users, or a delivery target.

### `labs/gitlab-training-<year>-<cohort-id>` (Project — template)

- **Purpose:** Live exercise workspace for one training cohort (e.g. `gitlab-training-2026-q3`).
- **What goes here:** Whatever that cohort's exercises produce. Created fresh per cohort rather than reused/wiped between sessions — cleaner access control and no reset process to maintain. Archive (don't delete) once the session wraps. The curriculum itself lives in `common/gitlab-training`, not here.

---

## `playground` (Group)

**Purpose:** Individual, unsupported experimentation.
**What goes here:** One subgroup per person (`playground/<username>`).

### `playground/<username>` (Group — one per team member)

- **Purpose:** One person's namespace for their own experimentation.
- **What goes here:** That person's own project(s) — see below.

#### `playground/<username>/<repo>` (Project — template)

- **Purpose:** Individual, unsupported experimentation.
- **What goes here:** One person's scratch work. Not maintained. No one else should build a dependency on it.

---

## `rfis` (Group)

**Purpose:** One-off responses to specific data requests (RFIs) — not ongoing products.
**What goes here:** One project per request.

### `rfis/rfi-<year>-<sequence>-<short-slug>` (Project — template)

- **Purpose:** One-off response to a specific data request.
- **What goes here:** The query/script that answered the request, the delivered output (if kept in-repo), and a README recording who asked, what was asked, what was delivered, and the source data version used (e.g. sddb release tag). Archive (don't delete) once roughly a year old with no activity, pending records-management confirmation on formal retention requirements. Graduates to `labs/` or `products/` if the same request keeps recurring.

---

*This listing reflects the structure as currently designed. Update it whenever a new
group or project is added so it doesn't go stale — an out-of-date reference document
causes the same confusion it's meant to prevent.*
