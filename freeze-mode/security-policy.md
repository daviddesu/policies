# Security Policy — Freeze Mode for Jira

**Effective date:** 1 July 2026

Freeze Mode for Jira ("the app") is developed and operated by David Lee
("I", "me"), an independent developer. This document describes how the app is
secured, how it handles your data, and how to report a security concern. The
short version: **the app runs entirely on Atlassian Forge, holds only the
admin-grade permissions its freeze mechanism requires, gates every privileged
action on a verified global-admin check, stores nothing outside Atlassian's
infrastructure, and keeps a tamper-evident audit log.**

## Architecture and hosting

The app is built entirely on **Atlassian Forge** and runs inside Atlassian's
cloud infrastructure. There are no external servers, no third-party services,
and no network egress outside Atlassian:

- All backend logic runs in Forge's managed runtime (FaaS); I operate no
  servers and have no infrastructure to patch or breach.
- The app makes **no network calls outside Atlassian** — no analytics, no
  third-party processors, no external APIs.
- All authentication is handled by the Forge runtime. The app never sees,
  stores, or transmits credentials, tokens, or API keys.

## Data handling

- **What is stored:** the durable **Freeze Log** and the active-freeze state
  needed to release a project. Per freeze this includes the freezing admin's
  Atlassian account ID, the target project key, freeze/release timestamps, a
  pointer to the project's original permission scheme (kept so it can be
  restored on release), the optional Exemption Group name, any transitions
  detected during the window, any release anomaly, and a free-text
  reason/reference. See the [Privacy Policy](./privacy-policy.md) for the full
  list. The app stores **no Jira issue content**.
- **Where it lives:** in **Atlassian Forge hosted storage** (Forge KVS),
  scoped to your Jira site's installation of the app. Data never leaves
  Atlassian's infrastructure and I have no access to it.
- **In transit:** all communication is between Forge and the Jira product APIs
  over Atlassian's encrypted transport; the app introduces no transport of its
  own.
- **Retention:** the Freeze Log is append-only and kept for the life of the
  installation. **Uninstalling the app permanently and automatically deletes
  all stored data, including the Freeze Log** — so the log is exportable
  (CSV/PDF) and the exported file, retained externally, is the durable record.

## Permissions and least privilege

The app runs at Jira's permission layer, so it necessarily requests
admin-grade scopes. Each is present for one specific reason, and no more:

- `read:jira-work` — read a project's current permission scheme and grants,
  list company-managed projects for the freeze picker, and query issue
  changelog to detect transitions during a freeze (Transition Reconciliation).
- `read:jira-user` — validate the optional Exemption Group and list groups for
  the picker.
- `manage:jira-configuration` — create, read, and delete the locked-down
  permission scheme used to enforce a freeze.
- `manage:jira-project` — associate the locked-down scheme with the project
  (the freeze) and restore the original scheme on release.
- `storage:app` — persist the Freeze Log and active-freeze state in Forge KVS.

The app requests **no scope to read, write, or delete Jira issue content**, and
does **not** declare `allowImpersonation`. New scopes will not be added without
updating this policy and the privacy policy first.

## Authentication and authorization model

All scheme-mutating Jira calls run as the app's own identity (`asApp()`), a
deliberate design choice recorded in ADR-0001: the app's granted scopes are the
capability, and a **verified global-admin check is the sole authorization
gate.** Because those scopes are powerful, the app treats that check as a
security-critical invariant:

- **Every** freeze, release, Freeze Log read, and export entry point
  re-verifies server-side that the invoking user holds *Administer Jira*
  (global admin), using the user's own account ID from the Forge invocation
  context, **before any scheme change or log access.**
- The check lives in the framework-free `FreezeAdminService`, which rejects any
  non-admin caller before the freeze mechanism runs.
- **UI placement is not relied on as a security control.** The admin console is
  a `jira:adminPage`, but the resolver re-verifies global-admin itself rather
  than trusting that the surface is admin-only.

| Action | Verified against the user |
|---|---|
| Freeze / Release / list Freeze Log / export | Verified **global Jira admin** (*Administer Jira*) |
| Read freeze status (issue-context indicator) | **Not gated** — freeze status is readable by anyone who can browse the project (by design). The admin-only reason is **never** exposed on this path |

These checks are **enforced in the backend resolver and service, never only in
the UI; closed by default** (any ambiguous or non-authorized result denies the
action); and **covered by unit tests, including denial paths.**

A structural consequence reinforces the model: the locked-down scheme
**strips `ADMINISTER_PROJECTS`**, so a project admin cannot change the scheme to
self-unfreeze. Only a global admin can release. The freeze holds against exactly
the people whose work is being frozen.

## Application security controls

- **Input validation:** client payloads are validated and type-checked
  server-side at the resolver boundary; project keys and identifiers are
  format-validated before use.
- **No injection sinks:** no SQL (Forge key-value storage only, with
  app-constructed keys), no shell execution, no `eval`/dynamic code. REST URL
  interpolation is percent-encoded; the transition-detection JQL uses only a
  format-validated project key.
- **No XSS surface:** the UI uses Forge UI Kit components only — no raw HTML,
  no `dangerouslySetInnerHTML`, no direct DOM manipulation.
- **Audit-log integrity:** Freeze Log entries are **append-only** (the app
  exposes no edit and no delete), timestamps are **server-assigned**, and
  entries are **hash-chained** — each carries a hash of the prior entry — so
  out-of-band tampering is **detectable on export**.
- **No sensitive logging:** the app performs **no application logging** (there
  are no logging statements in the codebase). It does not write account IDs,
  project keys, or freeze reasons to logs.

## Security model and honest limitations

A Freeze is enforcement at Jira's permission layer, not tamper-proof
immutability. Stated plainly:

- **Site administrators** (*Administer Jira*) and **admin-context automation
  and apps** can still bypass project permissions. A freeze means "read-only
  for normal project work," not that no change can possibly occur.
- **Transitions are best-effort blocked and fully detected**, not guaranteed
  prevented: some workflow-condition-gated transitions may occur, and the app
  records them in the Freeze Log (Transition Reconciliation) rather than
  guaranteeing prevention.
- **The Freeze Log is tamper-evident, not tamper-proof** against the app's own
  storage, and is destroyed on uninstall — which is why export is the system of
  record.

These are deliberate v1 boundaries, documented so the app's guarantees are not
overstated.

## Vulnerability reporting

If you believe you have found a security vulnerability in the app, please
report it privately to **support@davidleetech.uk** with the subject line
"Security — Freeze Mode". Please include:

- a description of the issue and its potential impact,
- steps to reproduce, and
- any relevant version or environment details.

Please do **not** disclose the issue publicly until it has been addressed. I
will acknowledge reports within **5 business days**, keep you informed of
progress, and credit reporters who wish to be acknowledged. As an independent
developer I do not operate a paid bug-bounty program.

For vulnerabilities in the underlying Atlassian Forge platform, please use
[Atlassian's security channels](https://www.atlassian.com/trust/security).

## Changes to this policy

If a future version of the app changes its security posture (for example, new
permissions or data handling), this policy will be updated and the effective
date revised before that version is released.

## Contact

Security or privacy questions: **support@davidleetech.uk**
