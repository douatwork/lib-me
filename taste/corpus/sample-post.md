---
category: taste/corpus
tags: [sample, post, nonfiction, technical, practitioner]
last_updated: 2026-06-09
agent_action: "Neutral demonstration of the practitioner register — imperative, scannable, opinionated with the why inline, convention-first. Replace with your own when you adopt lib-me."
---

<!--
Demonstration of the nonfiction/technical register: practitioner writing for
practitioners. Note the imperative voice, scannable structure, rules-with-rationale,
and convention-first specifics (exact group names, exact settings) with a dry edge.
Neutral example; safe for few-shot use.
-->

# Adhoc Reporting

Any reporting produced by authors other than the Data Team.

## Guiding principals

- Ad-hoc content is not supported by the Data Team.
- Ad-hoc authors can publish reports to designated workspaces.
- The contents of those workspaces may be distributed via apps with approval.
- Icons for ad-hoc apps and workspaces should use white PowerPoint icons and have
  a gold background to visually distinguish them from official content.
- Ad-hoc authors may have Contributor + update app permissions on workspaces.
- No one outside the Data Team may have Admin or Member permissions on a workspace.

## Set up

- Create two AD groups
  - `powerbi-role-poweruser-[workspace]` — members = everyone who will publish
  - `powerbi-distro-[workspace]` — members = everyone who can view
- Create a well-named workspace.
- Add the group `powerbi-role-dev` as admin.
- In Workspace Settings > Advanced > Security Settings, check the box to allow
  Contributors to update apps.
- You do not need to know the members of these two groups to create and use them.
- Add group `powerbi-role-poweruser-[workspace]` as **Contributors**.
- Once they have published something, create the app. Contributors cannot create
  apps, but earlier we modified settings to allow them to update the app contents.
  This lets ad-hoc authors choose which reports are included and update the app
  themselves when they add or remove content.
- Give app permissions to `powerbi-distro-[workspace]`.
- Set the app color to Green.
- Set the app to install automatically.
- DO NOT provide BUILD or SHARE permissions.

## Communicate

- Advise the power users that their workspace is ready.
- Ask if they need long-term distribution for their reports.
  - If yes, explain about apps and offer to set theirs up if appropriate.
- Ask them to let you know if the workspace becomes disused so it can be removed.
- Offer to assist with workspace and distribution issues.
