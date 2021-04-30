# Using-Managed-Labels-to-Track-Issues

We reserve a set of labels for use with Github issues, to assist with issue handling and project management for OpenLibrary.

Most generally, an issue evolves through a series of states, for example:

submitted -&gt; assessed -&gt; scheduled -&gt; fixed -&gt; closed.

On the way down that path, lots of things can happen. The labels below can be applied and removed during initial triage \(either by the submitter, or by an initial triage\), and thereafter by the owner of the bug.

The sections below contain:

* [Owners](using-managed-labels-to-track-issues.md#owners)
* [Milestones](using-managed-labels-to-track-issues.md#milestones)
* [Managed Labels By Group](using-managed-labels-to-track-issues.md#managed-labels-by-group)
  * [Type](using-managed-labels-to-track-issues.md#type)
  * [Priority](using-managed-labels-to-track-issues.md#priority)
  * [State](using-managed-labels-to-track-issues.md#state)
  * [Needs](using-managed-labels-to-track-issues.md#needs)
  * [Close](using-managed-labels-to-track-issues.md#close)
  * [Theme](using-managed-labels-to-track-issues.md#theme)
  * [Affects](using-managed-labels-to-track-issues.md#affects)
  * [Module](using-managed-labels-to-track-issues.md#module)
  * [Additional Lables](using-managed-labels-to-track-issues.md#additional-labels)
* [Personal Labels, Deprecated Labels](using-managed-labels-to-track-issues.md#personal-labels-and-deprecated-labels)
* [Tips for Filtering Issues and Updating Their Labels](using-managed-labels-to-track-issues.md#tips-for-filtering-issues-and-updating-their-labels)

## Owners

The assignee of an issue is considered its _owner_.

At submission and prior to triage, the assigned owner is _not necessarily the person_ who will fix the issue \(it is not necessarily even established, at that point, if or when the issue will be fixed at all\), but rather they are the person who will do as much or as little as needed to handle the issue \(asking questions, soliciting input, establishing and updating the priority, checking if it is a duplicate, etc\).

Once an issue is labeled `State: Work In Progress`, the owner is the individual doing the work, or leading/coordinating the group that is doing the work.

Most parts of the project have _leads_; issues in their area typically get assigned to them first \(but can subsequently be handed off\). The submitter of an issue is free to suggest an owner — indeed, developers occasionally create issues which they assign to themselves\).

See the [Team Leads Table](https://github.com/internetarchive/openlibrary/wiki#team-leads-table) to get an idea of who to tag.

An example of a common [search](using-managed-labels-to-track-issues.md#tips-for-filtering-issues-and-updating-their-labels) might be [`assignee:cdrini label:"state: work in progress"`](https://github.com/internetarchive/openlibrary/issues?utf8=%E2%9C%93&q=assignee%3Acdrini+label%3A%22state%3A+work+in+progress%22) to see what Drini is working on.

Singular ownership is important to make sure things don't fall on the floor. We therefore avoid multiple assignees. Most issues have multiple individuals involved in various aspects of assessing and resolving it - those people are "mentioned" \(e.g. "@hornc"\) in the issue comments.

Any open bug that is unowned is in need of triage.

## Milestones

Much of our work is done by volunteers. So we plan very lightly, by year and by quarter, given the resources we have available. As new volunteers join, plans may change to accommodate their skills and interests.

Things we'd like to get done during a given year get the milestone for that year, e.g. "2019". For things we expect to be done in by the end of a given quarter, we apply the quarterly milestone, e.g. "2019 Q1" for things to get done on or before March 31, 2019.

Milestones are closed when their deadline arrives. Issues associated with that milestone that are not done get rescheduled \(the milestone is changed\) or [backlogged](using-managed-labels-to-track-issues.md#state) \(labeled `State: Backlogged`\).

## Managed Labels By Group

Labels are grouped by prefix and color. If you create a label outside the managed set, prefix it with your initials and give your personal labels a common color. We are continually evolving the managed set to meet our needs. If you think a label deserves to be in the managed set, just mention it.

The labels are grouped into different axes for slicing and dicing issues:

### Type:

What kind of issue this is. Is it something that is broken that should \(perhaps\) be fixed, or is it a request for a new feature or enhancement, or is it a reminder to reorganize or clean up some aspect of the code base?

| Color | Label | Description |
| :--- | :--- | :--- |
| ![ff9900](https://via.placeholder.com/30x30/ff9900/ff9900.jpg) | **Type: Bug** | Something isn't as intended. \[managed\] |
| ![ff9900](https://via.placeholder.com/30x30/ff9900/ff9900.jpg) | **Type: Feature** | Issue describes new functionality we'd like to implement. \[managed\] |
| ![ff9900](https://via.placeholder.com/30x30/ff9900/ff9900.jpg) | **Type: Question** | This issue doesn't require code. A question needs an answer. \[managed\] |
| ![ff9900](https://via.placeholder.com/30x30/ff9900/ff9900.jpg) | **Type: Refactor/Clean-up** | Issues related to reorganization/clean-up of data or code \(e.g. for maintainability\). Specifically "restructuring of an existing body of code, altering its internal structure **without changing its external behavior**. \([https://refactoring.com](https://refactoring.com)\). \[managed\] |
| ![e08000](https://via.placeholder.com/30x30/e08000/e08000.jpg) | **Type: Epic** | A feature or refactor that is big enough to require subissues. \[managed\] |
| ![ff9900](https://via.placeholder.com/30x30/ff9900/ff9900.jpg) | **Type: Subtask of Epic** | A subtask that is part of the work breakdown of an epic issue \(see comments\). \[managed\] |

Epics and subtasks are used when we want to separate out the ownership, comment stream, and timing of different parts of a large project. The Epic is closed when all its subtasks have been closed. For most issues, putting a checklist in the comment stream suffices \(when everything is checked off, the issue can be closed\). The "Needs: Breakdown" label can be used for any issue \(epic or not\) that needs a decision identifying the list of steps that will be taken in order to close the issue.

Note that `Bug`, `Feature`, `Question`, `Refactor`, `Subtask` are mutually exclusive. Every issue \(post-triage\) should have one of these. If an issue is labeled `Epic`, it probably also should have a `Feature` or `Refactor` label.

### Priority:

Priority describes how urgent the bug is. Very urgent bugs generally have an active conversation on the slack channel, so that they can be fixed right away.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![d12f29](https://via.placeholder.com/30x30/d12f29/d12f29.jpg) | **Priority 0: Immediate** | Issues that prevent users from using the site, or that corrupt site data. \[managed\] |
| ![ff8899](https://via.placeholder.com/30x30/ff8899/ff8899.jpg) | **Priority 1: Urgent** | Do this week, receiving emails, time sensitive. \[managed\] |
| ![ff8899](https://via.placeholder.com/30x30/ff8899/ff8899.jpg) | **Priority 2: High** | Important, as time permits. \[managed\] |
| ![ff8899](https://via.placeholder.com/30x30/ff8899/ff8899.jpg) | **Priority 3: Normal** | Issues that we can consider at our leisure. \[managed\] |
| ![ff8899](https://via.placeholder.com/30x30/ff8899/ff8899.jpg) | **Priority 4: Low** | An issue, but should be worked on when no other pressing work can be done. \[managed\] |

When a priority label is applied to an issue by the submitter, or on any issue without an owner, it represents a suggestion, not a decision. Priorities are not immutable - even while an issue is being worked on, the owner may decide to move the priority up or down.

Although priorities indicate urgency rather than timing, a helpful frame for assigning priority is to ask questions such as these - think of them as "rules of thumb":

* Is the issue preventing numerous users from successfully using the website? Is the issue related to leaking sensitive information? Is the issue related to usage or running processes actively corrupting our data? Does it really need to be fixed in the next 48 hours? Mark it `Priority 0: Urgent`. If it requires conversation or multiple developers to fix, they should all be talking in a slack channel right now.
* Should a developer interrupt and set aside their current activity to get this issue resolved? Does it need to be fixed in the next 14 days? Assign an owner who will label it `Priority 1: High`. If you can't find a volunteer to own it, it can't be `Priority 1`.
* The other priority labels are available to memorialize your assessment. They mostly indicate, "I've looked at this, and it is not urgent."

At most, one `Priority` label can be assigned to an issue. If there is no `Priority` label, consider the priority unassessed. It is good form to mark all your issues with some priority level, because it gives us a historical record of the distribution of issues by priority.

### State:

Use these labels to distinguish between issues that we're actively working on, those that we plan to work on, and those that seem to be good ideas that we'll consider when we have the additional time and resources required.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![e07cf9](https://via.placeholder.com/30x30/e07cf9/e07cf9.jpg) | **State: Backlogged** | No one working on it, not in any milestone, but want to leave open to consider later. \[managed\] |
| ![e07cf9](https://via.placeholder.com/30x30/e07cf9/e07cf9.jpg) | **State: Blocked** | Progress has stopped, we are waiting for something. \[managed\] |
| ![e07cf9](https://via.placeholder.com/30x30/e07cf9/e07cf9.jpg) | **State: Scheduled** | A decision has been made that this issue should be addressed. \[managed\] |
| ![e07cf9](https://via.placeholder.com/30x30/e07cf9/e07cf9.jpg) | **State: Work In Progress** | This issue is being actively worked on. \[managed\] |

If no state label is present, the issue needs assessment.

If someone was working on an issue but had to set it aside, the state label might be changed to "Backlogged," or the current owner might find someone to hand it off to, or it might even be closed \(if we decide it didn't need to be addressed after all\).

If an issue is "State: Scheduled", it must have a milestone that indicates by when it is scheduled to be completed. We plan by quarters, so "2019 Q1" means it is an issue we expect to resolve on or before March 31, 2019.

If an issue is `Priority 0` or `Priority 1`, and the state is not `Work In Progress`, something is wrong, and alarms should sound.

### Needs:

These labels indicate that an issue or pull request is stuck because the owner needs someone to respond - they'll add comments to the issue saying what exactly they need.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Triage** | This issue needs triage. The team needs to decide who should own it, what to do, by when. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Breakdown** | This big issue needs a checklist or subissues to describe a breakdown of work. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Community Discussion** | This issue is to be brought up in the next community call. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Detail** | Submitter needs to provide more detail for this issue to be assessed \(see comments\). \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Feedback** | A proposed feature or bug resolution needs community feedback prior to forging ahead. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Help** | Issues, typically substantial ones, that need a dedicated developer to take them on. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Review** | This issue/PR needs to be reviewed in order to be closed or merged \(see comments\). \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Special Deploy** | PR needs a non-standard deploy. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Submitter Input** | This issue/PR needs a response from the submitter. \[managed\] |
| ![0052cc](https://via.placeholder.com/30x30/0052cc/0052cc.jpg) | **Needs: Investigation** | This issue/PR needs a root-cause analysis to determine a solution. \[managed\] |

If you see one or more of these labels on an issue, assume we are not making progress on it.

If you are the owner of an issue and add this label, always add a comment that indicates, as best you can, what you need to get unstuck. If you think you need the help of another team member, make sure to mention them by their handle in the comment.

Remember to remove this label once the need is met and the issue is unstuck.

### Close:

Issues typically lead to pull requests to modify the repo in order to resolve the bug.  
It is considered good form, immediately prior to closing a bug, to add a label indicating if it was closed for any of the following reasons.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![cfd3d7](https://via.placeholder.com/30x30/cfd3d7/cfd3d7.jpg) | **Close: Duplicate** | This issue or pull request already exists \(see comments for pointer to it\). \[managed\] |
| ![cfd3d7](https://via.placeholder.com/30x30/cfd3d7/cfd3d7.jpg) | **Close: Not Reproducible** | Closed because we cannot reproduce the issue. \[managed\] |
| ![cfd3d7](https://via.placeholder.com/30x30/cfd3d7/cfd3d7.jpg) | **Close: Not an Issue** | Questions and discussions resolved or moved to gitter/slack. \[managed\] |
| ![cfd3d7](https://via.placeholder.com/30x30/cfd3d7/cfd3d7.jpg) | **Close: Will Not Fix** | Closed because we have decided not to address this \(e.g. out of scope\). \[managed\] |

Some observations:

* You'll almost always want to add additional detail in the comments as to how the decision to close was arrived at. Liberally mention other people you consulted with \(for example, "I spoke to @jeff and we agreed this affects very few users."\) and other issues \(super-common: "this is a duplicate of issue \#XXXX"\).
* One of the best ways to attract attention to an issue that you feel is unfairly ignored is to close it prematurely.
* If there is \(or should be\) another `State: Scheduled` issue to fix a big problem, and fixing the big problem would also resolve this issue, we have a choice: we could address this issue \(e.g. with a stop-gap solution, right now, while we wait for the big fix\), or close this issue \(i.e. decide to leave it broken\). In the comments, reference the big picture issue \(e.g. "This issue will be properly resolved when we address issue \#XXXX."\), and explain your intentions. For example, you might comment "We'll leave this broken until then," and label `Close: Will Not Fix`, or comment "In the meantime, we'll deploy the following stop-gap" and label this issue, say, `Priority 1: High`.

### Theme:

There are some issues that affect multiple modules, or are related to a user story or workflow that touches multiple systems, and we use "theme" labels to identify them. This list is expected to grow.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Accessibility** | Work related to disability accessibility. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Book Sponsorship** | Issues related to the workflow for book sponsorship. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Backup/Restore** | Issues related to disaster recovery, backup/restore, data dumps. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Design** | Issues related to UI design, branding, etc. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Development** | Issues related to the developer experience and the dev environment. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Identifiers** | Issues related to ISBN's or other identifiers in metadata. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Internationalization** | Making OpenLibrary work for both foreign-language users and books. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Performance** | Issues related to UI or Server performance. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Reading Log** | Related to workflows for creating, modifying, displaying a user's reading log. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Search** | Issues related to search UI and backend. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Upgrade to Python 3** | Issues relating to the systemwide upgrade from Python 2 to Python 3. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Testing** | Work related to tests that need to be written or fixed. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Translation** | Work related to language accessibility. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Public-APIs** | Issues related to APIs accessible to external parties. \[managed\] |
| ![2eb8db](https://via.placeholder.com/30x30/2eb8db/2eb8db.jpg) | **Theme: Editing** | Issues related to APIs accessible to external parties. \[managed\] |

The unifying characteristic of Themes is that they involve issues that touch many parts of the repo \(UI, Server, Configuration, Documentation, Data\).

It is expected that an issue will have at most one `Theme:` label.

### Affects:

The broad area this issue is related to, often suggesting who first should consider it.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Admin/Maintenance** | Issues relating to support scripts, bots, cron jobs and admin web pages. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Configuration** | Issues related to system configuration \(production, staging, or development\). \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Data** | Issues that affect book/author metadata or user/account data. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Documentation** | Issues related to developer or ops or data documentation. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Librarians** | Issues related to features that librarians particularly need. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Mobile/Responsive** | Affects the responsive UI on mobile devices. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: Server** | Issues with the server or its plugins. \[managed\] |
| ![fcbe9f](https://via.placeholder.com/30x30/fcbe9f/fcbe9f.jpg) | **Affects: UI** | The issue is focused on the web user interface and user experience. \[managed\] |

It is preferred that an issue only have one `Affects:` label, but we're not religious about it. If you notice that an issue affects multiple areas in the above list, you may want to split it into multiple issues, one per area. If it makes sense to resolve them independently, that's enough. If they all need to be resolved in a coordinated fashion, create an `Type: Epic` issue which can remain open until all the subissues are closed.

### Module:

These labels identify the specific module or service that the issue relates to. Often this corresponds to a particular directory or file or interface or class present in the repo hierarchy. This list is expected to grow.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Accounts** | Issues related to authentication, account maintenance, etc. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Docker** | Issues related to the configuration or use of Docker. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Git** | Issues related to the git repo, branches, commit messages, etc. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Infogami** | Issues related to the configuration or use of the Infogami subsystem. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: JavaScript** | Issues related to the JavaScript functionality. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Memcache** | Issues related to the configuration or use of the Memcache subsystem. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: Solr** | Issues related to the configuration or use of the Solr subsystem. \[managed\] |
| ![c2e0c6](https://via.placeholder.com/30x30/c2e0c6/c2e0c6.jpg) | **Module: CSS** | Issues related to CSS stylesheets. \[managed\] |

A common search might be something like `label:"Affects: Server" label:"Module: Solr" label:"State: Work In Progress"` to see who is actively working on calls to solr in the server. If you wanted to pick up issues in that area, you could see who else is doing so.

### Additional labels

A few remaining labels that are not in any group, because of github conventions, or for other reasons.

| Color | Label | Description |
| :--- | :--- | :--- |
| ![7057ff](https://via.placeholder.com/30x30/7057ff/7057ff.jpg) | **Good First Issue** | Easy issue. Good for newcomers. \[managed\] |

A `good first issue` should be clear, should not require a lot of context, should be low risk and easy to review. Issues that involve high priority or global changes to the production system code are not good candidates.

## Personal Labels and Deprecated Labels

If you have a large number of issues assigned to you, there may be times when you want to divide them into groups by your own criteria. You can do this by creating your own labels. They all go into the repository label set, so we ask that you:

* Pick your own unique \(!\) color that you apply to all your personal labels, and
* Prefix the label name with your initials.

For example, Charles Horn creates his own labels with prefix `CH:` and color \#1d76db.

Labels that are grey and/or start with a tilde `~` are _deprecated_. They typically are not used much, and shouldn't be added to issues going forward.

## Tips for Filtering Issues and Updating Their Labels

* The default [issues page](/internetarchive/openlibrary/issues) presents several search fields. There is one at the very top, for searching the repository or all of github. There is another one under the `issues` tab that searches through the issues. To the right of the issues filter is the labels button, which gives you a "labels list" to browse and drill down to issues. ![Issues Page Snippet](https://user-images.githubusercontent.com/6502462/57257705-7bece100-705a-11e9-8272-3879419dedd3.png)
* You can type a label search term directly into the issues filter search field, you type `label:<labelname>` to include issues containing the label, and `-label:<labelname>` to exclude issues containing that label. Because all the managed label names contain spaces and colons, you need to quote them if you are typing them directly into the issue search bar. For example, `label:"affects: documentation"` \(case doesn't matter\).
* If the issues filter field contains multiple labels terms, you will see the issues that match _all_ those label terms. There is no way to see a list issues that contain label A _or_ label B.
* If you're not sure exactly what the label is, just use the labels pull down, and in its search bar type any word that is in the label name or description, and the matches will be displayed. Click on the labels you want to add to your issues filter.
* All the managed labels have `[managed]` in their description, so an easy way to browse just the managed labels is to search labels on that string.
* It is easy \(and sometimes dangerous\) to do batch updates of labels \(or milestone, or assign, or close\). Just filter the issues to the ones you want to change, select the issues you want to label. When you select one or more issues, the pulldown menu changes from filter mode to update mode. Pull down the label menu, and you can select which labels you want to add or remove \(it's a toggle\).  ![issues list in update mode](https://user-images.githubusercontent.com/6502462/57258627-439ad200-705d-11e9-8cca-adf8e45f44d0.png)

