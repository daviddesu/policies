# Privacy Policy — Freeze Mode for Jira

**Effective date:** 1 July 2026

Freeze Mode for Jira ("the app") is developed and operated by David Lee
("I", "me"), an independent developer. This policy describes what data the
app handles and where it lives. The short version: **your data never leaves
Atlassian's infrastructure, and I never see it.**

## What the app does

The app lets a global Jira administrator put one or more company-managed
projects into a temporary read-only ("frozen") state during sensitive
windows — audits, migrations, financial close — and cleanly release them
afterward. It works entirely at Jira's permission layer, by swapping a
project's permission scheme for a locked-down one and restoring it on
release. It also keeps a durable **Freeze Log**: an append-only record of
each freeze and release, which is the app's core compliance artifact.

## What data the app stores

When a project is frozen or released, the app stores a Freeze Log entry and
the active-freeze state needed to release it later. Per freeze this includes:

- The Atlassian account identity of the administrator who performed the
  freeze or release.
- The target Jira project (its key/identifier) and the freeze and release
  timestamps.
- A pointer to the project's original permission scheme (stored so the app
  can restore it on release, and mirrored into the log as a recovery
  artifact).
- The name of the optional Exemption Group, if one was enabled for the
  freeze (the group name only — never its membership or activity).
- Any transitions detected on the project during the freeze window
  (**Transition Reconciliation**), which may include issue keys and the
  Atlassian account identity of whoever performed the transition.
- Any release anomaly (for example, scheme drift or a missing original
  scheme) and its recovery outcome.
- A free-text reason/reference entered by the administrator (for example, an
  audit ticket number). This may contain personal data if the administrator
  types it there.

Entries are hash-chained (each carries a hash of the prior entry) so
out-of-band tampering is detectable on export. That is the only data the app
persists.

## Where the data lives

The Freeze Log and active-freeze state are stored in **Atlassian Forge
hosted storage** (Forge KVS), inside Atlassian's cloud infrastructure,
scoped to your Jira site's installation of the app. The app is built
entirely on Atlassian Forge and makes **no network calls outside
Atlassian**:

- No data is ever transmitted to me or to any third party.
- There are no external servers, no third-party processors, no analytics,
  no advertising, and no tracking of any kind.
- I have no access to the Freeze Log or any data your site stores.

Atlassian's handling of Forge-hosted data is governed by the
[Atlassian Privacy Policy](https://www.atlassian.com/legal/privacy-policy).

## What data the app accesses but does not store

To do its job, the app reads from and configures your Jira site at the
moment an administrator uses it, under the permissions granted at install
time:

- **Permission schemes** — it reads a project's current permission scheme
  and its grants, and creates, associates, and deletes the locked-down
  scheme used to enforce a freeze (`read:jira-work`,
  `manage:jira-configuration`, `manage:jira-project`).
- **Projects** — it lists your company-managed projects to populate the
  freeze picker (`read:jira-work`).
- **Groups** — it validates the optional Exemption Group and lists groups
  for the selection dropdown (`read:jira-user`).
- **Issue transition history** — during a freeze window it queries issue
  changelog data to detect transitions that occurred, for Transition
  Reconciliation (`read:jira-work`). Only the resulting detected-transition
  records described above are retained; the underlying issue content is not.

These reads and configuration changes happen inside your Jira site under the
permissions you granted at install time and are not retained by the app
beyond the Freeze Log described above. The app requests **no permission to
read, edit, or delete issue content** — it operates on permission schemes,
not on the substance of your issues.

## Retention and deletion

- The Freeze Log is append-only and kept for the life of the installation:
  the app exposes no edit and no delete for log entries, and timestamps are
  server-assigned.
- Because **uninstalling the app permanently deletes all Forge-hosted
  storage, automatically**, uninstalling also destroys the in-app Freeze
  Log. For that reason the log is **exportable (CSV/PDF)** and export is
  prompted at release: the exported file, retained outside the app, is the
  durable system of record. Nothing is retained by the app after uninstall.

## Your rights

Because all data is stored within your own Jira site's Forge storage and
under your organisation's Atlassian agreement, your Jira admin controls it
fully: any stored data is removed by uninstalling the app. For data-subject
requests concerning your Atlassian account data, see Atlassian's privacy
resources.

## Changes to this policy

If a future version of the app changes what data is handled (for example,
optional features that require new permissions), this policy will be
updated and the effective date revised before that version is released.

## Contact

Questions or concerns: **support@davidleetech.uk**
