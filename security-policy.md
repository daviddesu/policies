# Security Policy — Snapshot Templates for Jira

**Effective date:** 16 June 2026

Snapshot Templates for Jira ("the app") is developed and operated by David Lee
("I", "me"), an independent developer. This document describes how the app is
secured, how it handles your data, and how to report a security concern. The
short version: **the app runs entirely on Atlassian Forge, holds the minimum
permissions it needs, stores nothing outside Atlassian's infrastructure, and
checks your permissions on every action.**

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

- **What is stored:** when you save a template, the app stores a snapshot of
  the issues you chose to capture (field content, the captured assignee's and
  creator's Atlassian account IDs, and the project ID). This is the only data
  the app persists. See the [Privacy Policy](./privacy-policy.md) for the full
  list.
- **Where it lives:** in **Atlassian Forge hosted storage**, scoped to your
  Jira site's installation of the app. Data never leaves Atlassian's
  infrastructure and I have no access to it.
- **In transit:** all communication is between Forge and the Jira product APIs
  over Atlassian's encrypted transport; the app introduces no transport of its
  own.
- **Retention:** templates persist until their creator deletes them.
  **Uninstalling the app permanently and automatically deletes all stored
  templates.**

## Permissions and least privilege

The app requests only three Jira scopes:

- `read:jira-work` — to read the issues you snapshot,
- `write:jira-work` — to recreate issues when you instantiate a template,
- `read:jira-user` — to check whether a captured assignee can be re-applied.

It deliberately does **not** request `delete:jira-work` — **the app cannot
delete Jira issues under any circumstances.** It does not declare
`allowImpersonation`. New scopes will not be added without updating this policy
and the privacy policy first.

## Authentication and authorization model

All Jira calls run as the app's own identity (`asApp()`), a deliberate design
choice recorded in ADR-0002. Because Jira does not automatically enforce the
invoking user's permissions under `asApp()`, **every privileged action is
gated server-side** by an explicit user-scoped permission check
(`POST /rest/api/3/permissions/check`, the API underlying Forge's Authorize
wrapper), using the invoking user's own account ID:

| Action | Verified against the user |
|---|---|
| Snapshot / read | `BROWSE_PROJECTS` on the epic and each child (issue-level security respected — issues hidden from the user are excluded) |
| Instantiate / retry | `CREATE_ISSUES` in the target project |
| Delete template | Template creator, or `ADMINISTER_PROJECTS`, resolved against the user |

These checks are:

- **enforced in the backend resolvers**, never only in the UI;
- **closed by default** — any non-2xx or ambiguous response denies the action;
- **covered by unit tests**, including permission-denial paths.

A template can only be instantiated into the same project it was captured from,
and target issues and projects are derived from the Forge invocation context,
never from client-supplied identifiers.

## Application security controls

- **Input validation:** all client payloads are validated and type-checked
  server-side; context identifiers are format-validated (issue-key pattern,
  numeric project ID) at the resolver boundary.
- **No injection sinks:** no SQL (Forge key-value storage only, with
  app-constructed keys), no shell execution, no `eval`/dynamic code. Every REST
  URL interpolation is percent-encoded; the single JQL query uses only a
  format-validated issue key.
- **No XSS surface:** the UI uses Forge UI Kit components only — no raw HTML,
  no `dangerouslySetInnerHTML`, no direct DOM manipulation.
- **No sensitive logging:** the app performs no application logging. The only
  output that can reach Forge logs is thrown error messages, restricted by
  construction to non-sensitive technical identifiers (issue keys, project IDs,
  template IDs, HTTP status codes) — never field content, account IDs, or
  credentials.

## Vulnerability reporting

If you believe you have found a security vulnerability in the app, please
report it privately to **support@davidleetech.uk** with the subject line
"Security — Snapshot Templates". Please include:

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
