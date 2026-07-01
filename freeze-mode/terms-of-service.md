# Terms of Service — Freeze Mode for Jira

**Effective date:** 1 July 2026

Freeze Mode for Jira ("the app") is developed and operated by David Lee
("I", "me", "the developer"), an independent developer. These Terms of
Service ("Terms") govern your use of the app. By installing or using the
app you agree to these Terms. If you do not agree, do not install or use
the app.

The app is distributed through the Atlassian Marketplace and runs on
Atlassian Forge. Your use of Jira and of the Atlassian Marketplace is also
governed by
[Atlassian's Cloud Terms of Service](https://www.atlassian.com/legal/cloud-terms-of-service)
and
[Marketplace Terms of Use](https://www.atlassian.com/licensing/marketplace/termsofuse).
Nothing in these Terms overrides your agreement with Atlassian.

## What the app does

The app lets a global Jira administrator put one or more company-managed
projects into a temporary read-only ("frozen") state and cleanly release
them afterward, and it keeps a durable, append-only **Freeze Log** as a
record of each freeze and release. It operates entirely at Jira's
permission layer by swapping a project's permission scheme for a
locked-down one and restoring it on release.

## Licence

Subject to these Terms and to any purchase made through the Atlassian
Marketplace, you are granted a non-exclusive, non-transferable, revocable
right to install and use the app within your own Atlassian Jira site for
your internal business purposes, for as long as it is installed. You may
not copy, modify, reverse-engineer, resell, or redistribute the app, except
to the extent that restriction is prohibited by applicable law.

## Fees

Pricing, billing, taxes, refunds, and any free trial or free tier are
handled by Atlassian through the Marketplace under Atlassian's terms. Any
fees are as displayed on the app's Marketplace listing at the time of
purchase.

## Acceptable use

You are responsible for how the app is used within your Jira site,
including who you grant Jira administrator rights to. You agree not to use
the app to violate any law, to interfere with the app's operation, or to
attempt to circumvent its authorization checks. Only a global Jira
administrator may initiate a freeze or a release; you are responsible for
managing that administrator access appropriately.

## Nature and limits of the service — please read

The app is a control that operates within the boundaries of Jira's
permission model. It is important that you understand what it does and does
not guarantee:

- **Freeze is read-only enforcement, not tamper-proof immutability.** A
  freeze blocks normal project write actions by swapping the permission
  scheme. Site administrators (holders of *Administer Jira*) and
  admin-context automation and apps can still bypass project permissions.
  A freeze means "read-only for normal project work," not that no change
  can possibly occur.
- **Transitions are best-effort blocked and fully detected, not
  guaranteed prevented.** Some workflow-condition-gated transitions may
  still occur during a freeze. The app detects and records transitions that
  happen during the window in the Freeze Log (Transition Reconciliation)
  rather than guaranteeing prevention.
- **Release is best-effort restore.** The app restores the original
  permission scheme only when that scheme still exists and the project is
  still on the scheme the freeze created. If the original scheme was
  deleted, or the project was changed through Jira's own admin tools while
  frozen, the app surfaces the anomaly and provides a guided manual-recovery
  path rather than blindly overwriting.
- **The Freeze Log is tamper-evident, not tamper-proof, and is destroyed on
  uninstall.** Entries are append-only, hash-chained, and server-timestamped
  so out-of-band tampering is detectable on export, but they are not
  cryptographically guaranteed against the app's own storage. Because
  Atlassian Forge deletes app storage on uninstall, the in-app log is a live
  convenience view. **You are responsible for exporting the Freeze Log
  (CSV/PDF) and retaining it externally as your durable record.**
- **Not legal or compliance advice.** The app helps you administer and
  document freeze windows. It does not itself make you compliant with any
  law, regulation, audit standard, or contractual obligation, and using it
  is not a substitute for your own legal, audit, or compliance judgment.

## Data and privacy

The app's handling of data is described in the
[Privacy Policy](./privacy-policy.md). All data the app stores lives in
Atlassian Forge hosted storage inside your Jira site; the developer has no
access to it and it is never transmitted outside Atlassian.

## Availability and changes

The app is provided on an ongoing basis but may be updated, changed, or
have features added or removed over time, including to reflect changes in
Atlassian Forge or the Jira platform. Reasonable effort will be made to
maintain availability, but no specific uptime or support-response time is
guaranteed under these Terms.

## Warranty disclaimer

The app is provided **"as is" and "as available," without warranties of any
kind**, whether express or implied, including but not limited to implied
warranties of merchantability, fitness for a particular purpose, and
non-infringement. The developer does not warrant that the app will be
uninterrupted, error-free, or that it will prevent any particular action,
change, or unauthorized access within your Jira site. To the extent any
warranty cannot be disclaimed as a matter of law, it is limited to the
minimum permitted by that law.

## Limitation of liability

To the maximum extent permitted by applicable law, the developer will not
be liable for any indirect, incidental, special, consequential, or punitive
damages, or for any loss of data, loss of profits, loss of goodwill,
business interruption, or failure to meet any compliance or regulatory
obligation, arising out of or relating to your use of, or inability to use,
the app — even if advised of the possibility of such damages. To the
maximum extent permitted by applicable law, the developer's total aggregate
liability arising out of or relating to the app and these Terms will not
exceed the greater of (a) the total fees you paid for the app through the
Atlassian Marketplace in the twelve (12) months before the event giving
rise to the liability, or (b) fifty US dollars (US$50).

## Indemnity

You agree to indemnify and hold the developer harmless from any claim,
demand, loss, or expense (including reasonable legal fees) arising out of
your misuse of the app or your breach of these Terms.

## Term and termination

These Terms apply for as long as the app is installed on your Jira site.
You may end them at any time by uninstalling the app. Uninstalling
permanently deletes the app's Forge-hosted storage, including the in-app
Freeze Log; export beforehand anything you need to retain. The developer
may suspend or terminate access if you materially breach these Terms or if
continued operation is no longer feasible. Sections that by their nature
should survive termination (including the warranty disclaimer, limitation
of liability, and indemnity) survive.

## Changes to these Terms

These Terms may be updated from time to time — for example, to reflect
changes in the app or in applicable law. Material changes will be reflected
by a revised effective date above. Your continued use of the app after an
update constitutes acceptance of the updated Terms.

## Governing law

These Terms are governed by the laws of England and Wales, and the courts
of England and Wales have exclusive jurisdiction, except where mandatory
local law provides otherwise. (If you operate under a different
jurisdiction, contact the developer.)

## Contact

Questions about these Terms: **support@davidleetech.uk**
