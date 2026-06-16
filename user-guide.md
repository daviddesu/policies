# Snapshot Templates for Jira — Documentation

Snapshot Templates lets you save an existing epic (or any single issue) and
its direct child issues as a reusable template, then recreate that whole
structure in your project in one click.

## Getting started

Install the app from the Atlassian Marketplace. No configuration is needed —
after install you'll find:

- **Save as template** in the ••• menu of every issue.
- **Templates** in every project's sidebar.

## Saving a template

1. Open the epic (or issue) you want to reuse and choose **••• → Save as
   template**.
2. The dialog shows what will be captured: the number of direct child
   issues and the list of captured fields — summary, description, issue
   type, priority, labels, components, assignee, and custom field values.
3. Give the template a name (it defaults to the epic's summary) and confirm.

Notes:

- An issue with no children is saved as a **single-issue template**.
- **Subtasks are not captured yet** — the dialog tells you when the issue
  has subtasks that will be excluded.
- Templates are capped at **50 child issues** and a stored size of 240 KiB.
  If a template is over either limit, the save fails with a clear message —
  nothing is ever silently truncated.

## Browsing templates

Open **Templates** in the project sidebar. It lists this project's
templates with their name, number of child issues, and creator.

## Creating issues from a template

1. On the Templates page, choose the template and select **Create**.
2. Edit the one pre-filled field — the new epic's summary — and confirm.
3. The app creates the new epic and all its children, with parent links
   wired up automatically.

Before creating, the app **pre-checks** the template against the project's
current configuration and warns you about anything that won't carry over —
for example a now-required field the template doesn't supply, or a captured
assignee who can no longer be assigned in the project (those issues are
created Unassigned).

Creation is **best-effort**: if some children fail to create (for example
due to a changed project configuration), everything that succeeded is kept
and the failed items are listed with Jira's reason. A **Retry failed**
action re-attempts only the failed children against the same epic — no
duplicates.

The person who creates from a template becomes the **reporter** of the new
issues, so the audit trail reflects who filed them.

## Deleting a template

Only the person who created a template can delete it, from the Templates
page.

## Scope and limits (v1)

- A template can be instantiated only into the **project it was saved
  from**.
- Templates capture **one level of hierarchy**: the epic/parent and its
  direct children. Subtasks are not captured yet.
- Maximum 50 children per template.

## Data, permissions, and privacy

- The app runs entirely on **Atlassian Forge**. Template data is stored in
  Forge hosted storage inside Atlassian's infrastructure and **never leaves
  Atlassian**.
- The app has **no delete permission** — it cannot delete Jira issues.
- **Uninstalling the app permanently deletes all stored templates.**
- See the [privacy policy](https://daviddesu.github.io/policies/privacy-policy).

## Support

Questions, bugs, or feature requests: **support@davidleetech.uk**
