# Privacy Policy — Snapshot Templates for Jira

**Effective date:** 11 June 2026

Snapshot Templates for Jira ("the app") is developed and operated by David Lee
("I", "me"), an independent developer. This policy describes what data the
app handles and where it lives. The short version: **your data never leaves
Atlassian's infrastructure, and I never see it.**

## What the app does

The app lets project members save a Jira epic (or a single issue) and its
direct child issues as a reusable template, and recreate that structure
later inside the same Jira project.

## What data the app stores

When you save a template, the app stores a snapshot of the issues you chose
to capture:

- Issue field content: summaries, descriptions, issue types, priorities,
  labels, components, and custom field values.
- The Atlassian account ID of the captured assignee (if any) and of the
  person who created the template.
- The ID of the Jira project the template belongs to.

That is the only data the app persists. It may include personal data if the
captured issues contain personal data (for example, a person's name in an
issue summary).

## Where the data lives

Templates are stored in **Atlassian Forge hosted storage**, inside
Atlassian's cloud infrastructure, scoped to your Jira site's installation
of the app. The app is built entirely on Atlassian Forge and makes **no
network calls outside Atlassian**:

- No data is ever transmitted to me or to any third party.
- There are no external servers, no third-party processors, no analytics,
  no advertising, and no tracking of any kind.
- I have no access to the templates your team stores.

Atlassian's handling of Forge-hosted data is governed by the
[Atlassian Privacy Policy](https://www.atlassian.com/legal/privacy-policy).

## What data the app accesses but does not store

To do its job, the app reads from your Jira site at the moment you use it:

- The issues you snapshot (to build the template).
- Display names of template creators (to show "created by" in the template
  list).
- The list of assignable users in a project (to check whether a captured
  assignee can be re-applied when a template is instantiated).

These reads happen inside your Jira site under the permissions you granted
at install time and are not retained by the app beyond the stored template
described above. The app requests read and write access to Jira work and
read access to user profiles. It deliberately requests **no delete
permission** — it cannot delete Jira issues.

## Retention and deletion

- A template is kept until the person who created it deletes it from the
  app's Templates page.
- **Uninstalling the app permanently deletes all stored templates,
  automatically.** Nothing is retained after uninstall.

## Your rights

Because all data is stored within your own Jira site's Forge storage and
under your organisation's Atlassian agreement, your Jira admin controls it
fully: any stored data can be removed by deleting templates or
uninstalling the app. For data-subject requests concerning your Atlassian
account data, see Atlassian's privacy resources.

## Changes to this policy

If a future version of the app changes what data is handled (for example,
optional features that require new permissions), this policy will be
updated and the effective date revised before that version is released.

## Contact

Questions or concerns: **davidcslee1990@gmail.com**
