# Freeze Mode for Jira — Documentation

Freeze Mode puts a Jira project into a controlled **read-only state** during
sensitive windows — audits, migrations, financial close — and cleanly
releases it afterward. It works at Jira's permission layer: freezing swaps the
project's permission scheme for a locked-down copy that **keeps existing view
access but removes every write grant**. The same people can still see the
project; nobody can change it.

## Who can freeze

Only a **global Jira admin** (a holder of *Administer Jira*) can freeze or
release a project. Project admins cannot freeze, and — importantly — cannot
lift a freeze on a project they administer. Every freeze and release entry
point is gated on a verified global-admin check.

## Getting started

Install the app from the Atlassian Marketplace. After install you'll find
**Freeze Mode** in Jira administration.

## Freezing projects

1. Open **Freeze Mode** and choose the projects to freeze. The picker lists
   **company-managed projects only** (see limits below).
2. Add a **reason / reference** (for example, the audit ticket). This is
   recorded in the Freeze Log and is visible only to admins.
3. Optionally name an **exemption group** — a single, pre-existing Jira group
   whose members keep write access during the freeze.
4. Confirm. Each selected project is frozen **independently**.

Freezing is **batch-friendly but not all-or-nothing**: if one project is
already frozen or errors, only that project is skipped — the rest freeze
normally. The result tells you which projects were frozen and which were
skipped.

## What a freeze blocks

During a freeze, the following are blocked for normal project work: editing
issues, commenting, attachments, worklogs, issue links, and issue creation
and deletion. Viewing and browsing are unchanged.

Workflow **transitions** are best-effort blocked and **fully detected**: any
transition that occurs on a frozen project during the window is surfaced in
the Freeze Log, so a leak becomes a documented event rather than a hidden gap.

A freeze is **read-only for normal project work, not tamper-proof
immutability** — global site-admins and admin-context automation can still
bypass project permissions. This is a documented limitation of the
permission-layer approach.

## The Freeze Indicator

Anyone who can browse a frozen project sees a **Freeze Indicator** panel on
its issues, telling them the project is frozen and who to contact. The
free-text reason is **not** shown here — it lives only in the admin-only
Freeze Log. This turns Jira's cryptic permission errors into a deliberate,
visible control.

## Releasing a project

Release returns a project to its prior editable state by **restoring its
original permission scheme**. v1 is **manual release only**.

Release is best-effort: it restores the original scheme when that scheme still
exists and the project is still on the locked scheme Freeze Mode created. If
the original scheme was deleted, or the project was moved to a different scheme
through Jira's own admin UI while frozen, Release does **not** blindly
overwrite — it surfaces the anomaly and drops into a **guided manual-recovery
path** where you choose the scheme to restore. The anomaly is recorded in the
Freeze Log.

## The Freeze Log

Every freeze and release is recorded in the **Freeze Log** — a durable,
append-only record that is your audit trail. Per freeze it captures: the admin
who froze, the target project, the freeze and release timestamps, the
exemption group (if any), the original-scheme pointer, any release anomaly,
and any transitions detected during the window.

The log is **tamper-evident**: the app exposes no edit and no delete for
entries, timestamps are server-assigned, and entries are **hash-chained** so
out-of-band tampering is detectable.

## Scope and limits (v1)

- **Company-managed projects only.** Team-managed (next-gen) projects manage
  their permissions independently and don't participate in shared permission
  schemes, so the freeze mechanism can't lock them.
- The **Project** is the unit of freeze — you cannot freeze individual issues,
  boards, or sprints.
- A project can be in **at most one active freeze** at a time; freezes do not
  stack.
- The **exemption group** is all-or-nothing in v1: exempt members regain the
  full write set, and the log records that the exemption existed and who was
  in scope — never what those members did.
- Release is **manual only** in v1.

## Data, permissions, and privacy

- The app runs entirely on **Atlassian Forge** and stores its data in Forge
  hosted storage inside Atlassian's infrastructure — nothing leaves Atlassian.
- **Uninstalling the app permanently deletes the stored Freeze Log.**
- See the [privacy policy](https://daviddesu.github.io/policies/freeze-mode/privacy-policy),
  [terms of service](https://daviddesu.github.io/policies/freeze-mode/terms-of-service),
  and [security policy](https://daviddesu.github.io/policies/freeze-mode/security-policy).

## Support

Questions, bugs, or feature requests: **support@davidleetech.uk**
