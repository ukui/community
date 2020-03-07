# Issue Manage

## Roles

In order to better manage issues, we divide participants into these roles:

| Role            | Functional authority                              |
| ---------------- | ------------------------------------------------- |
| Testers          | Test UKUI's projects.                             |
| Developer        | Development and maintenance of UKUI's projects.   |
| Community member | Community enthusiasts and participant.            |

## Issue categorization labels

In medium-scale or large-scale project, different participants have different levels of participation in the project. In order for each participant to find and resolve issues by filter, we decided to classify issues. In general, the UKUI's projects have 9 custom classification labels by default.

| Label                 | Description                                                                                                                      |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| kind/bug              | This issue is an unknown bug found by a community member.                                                                        |
| kind/documentation    | This issue is related to documentation.                                                                                          |
| kind/duplicate        | This issue is a duplicate bug found by a community member.                                                                       |
| kind/enhancement      | This issue is request adding new features or optimizing existing features.                                                       |
| kind/good-first-issue | This issue is helpful for developers or testers who are new to the project.                                                      |
| kind/help-wanted      | This issue is assignee needs help from other community member.                                                                   |
| kind/invalid          | This issue is invalid or unavailable.                                                                                            |
| kind/question         | This issue is questionable or needs more information.                                                                            |
| kind/wontfix          | This issue is request adding new features or fixes existing bug, but not planned to be implemented within the project schedule.  |

## Issue status labels

In fact, the impact of different issues on the project or the urgency to be resolved have different degree. In order to track the priority of the issue and the current status, UKUI has agreed on such useful labels.

### Priority labels

| Label           | Description                                                                                                                     |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| priority/high   | High-priority issues, need to be fixed as soon as possible.                                                                     |
| priority/medium | Medium-priority issues, non-critical issues.                                                                                    |
| priority/low    | Low-priority, non-critical and nonurgent, generally used with minor issues, or issues that need to be confirmed for scheduling. |

Color value (RGB) of priority labels:

* `priority/high`:  #ff0000
* `priority/medium`: #ff3366
* `priority/low`: #aa8899

### Status labels

| Label              | Description                                                                                           |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| status/confirmed   | The project maintainer has confirmed that the issue described in the issue exists.                    |
| status/resloved    | The project maintainer has fixed the issue described in the issue, and closes the issue after review. |
| status/reopen      | The issue have error again that Marked as reslove.                                                    |
| status/need-assign | The issue requires assigned principal.                                                                |

Color value (RGB) of status labels:

* `status/need-assign`: #ffff00
* `status/confirmed`: #00ffff
* `status/resolved`: #00ff00
* `status/reopen`: #ff6666

## Extend label

If project is added some new labels, the new labels should not be ambiguous with the existing labels, and the usage and specifications of the label should be explained, and updated to this document to ensure the unified perception of project participants.

## Issue workflow

### Tester workflow

1. For bugs found in the internal test, mark as `kind/bug` after submission, and indicate the priority (`priority/high`, `priority/medium`, or `priority/low` ) at once.
2. For issues committed by other people, add one of categorization labels(`kind/bug`, `kind/enhancement`, `kind/invalid`, `kind/documentation`) according to the described in the issue, and verify the bug existed. If problem is existed, evaluate its priority, add or modify the priority label.
3. If it is clear that can be assigned to someone directly in the issue, otherwise wait for the developer to reslove or the project leader to assign it.
4. For issues marked as `status/resolved` label, perform regression testing. If the acceptance is passed, close the issue. If it failed, the `status/resolved` label is replaced with `status/reopen`.
5. For the issue that has passed the acceptance to appear again, you need to reopen the issue and mark it with `status/reopen`.

### Developer workflow

1. Self-verify issue that marked as `kind/bug`, if the verification is passed, add `status/confirmed` label.
2. Select some issues marked as `status/confirmed` to solve(mean it will assigned to yourself).
3. For issues assigned to yourself, if you can't reproduce them or have questions, add `kind/question` label to let the issue committer provide more information.
4. If the assigned issue has been resolved, add `status/resolved` label to let the tester scheduling test. If you have a `status/reopen` issue that assigned to yourself, resolve it in time.
5. For issues that don't have time to deal with, add `kind/help-wanted` label.
6. Add `kind/wontfix` label for feature requirements or issues that will not be implemented within the project schedule.

### Community member workflow

1. Anyone who finds a problem or need help can report an issue.
2. Help bugs confirmation(simply reply in issue).
3. Responsible for reslove issues that are not assigned to assignee, and resolved for mark as `kind/help-wanted` issues as much as possible. For participant who are not in the ukui project or starter, they can just reply in the issue and getting started.