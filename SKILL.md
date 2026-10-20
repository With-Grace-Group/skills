---
name: withgrace
description: Use for With Grace team work through the With Grace connector, chiefly your own Asana tasks (due dates, Status, descriptions, owners) and the pages inside portal.wgrace.co. For the With Grace team only.
---

# With Grace

With Grace is an architecture and construction practice in Bali. This skill
connects Claude to the With Grace connector for people on the With Grace team.
It is not available to clients or villa buyers yet: if the connector lists only
`with_grace_access`, say that and stop.

Setup is in [README.md](README.md). If a tool named below is missing, say so
rather than guessing.

## What it is for today

1. **Your own Asana.** Read your tasks, tidy them, and keep the board easy to
   read at a glance.
2. **Work inside portal.wgrace.co.** Village pages, villa rooms, studio
   proposals, the CRM pipeline, room documents and ads production.

## Asana

Start with `asana_connection_status`. If it is not connected, call
`asana_connect` and give the person the link to approve. Every Asana tool acts
as that person, through their own connection.

| Tool | Use |
| --- | --- |
| `asana_my_tasks` | Your open tasks and the ones finished this week, each with its due date, Status and description |
| `asana_project_tasks` | One project's tasks, by Asana project id |
| `asana_update_task` | Set a due date, a Status, a description, an owner, or mark done |
| `asana_comment` | Add progress or a decision as a comment |
| `asana_task_comments` | Read a task's comments, oldest first |
| `asana_create_task` | Add a task |
| `asana_add_to_section` | Put an existing task on a second board |

**A tidy task has three things:** a due date for when it should be done, a
Status so people can see whether it has started, and a description that says
in one line what the task is. Progress and decisions go in a comment.

**Status is a field, not a board section.** Pass its option name, such as Todo
or In Progress, to `asana_update_task`. If the tool says the board has no
Status field, tell the person; adding the field is a board setting.

**Keep going, and ask before each save.** Run the checks and read the tasks
without stopping to ask. Stop only when the person must do something, such as
signing in, or to approve a change. Show five tasks at a time in a short table
(the task, what it is missing, your suggestion), save them all on one yes, then
go straight on to the next five. Never change a task the person has not
approved, and never wipe a description someone wrote.

**Finished or no longer needed?** Mark it complete with `completed: true`.

## Company procedures

Before work a company procedure covers, call `find_skill` with a short
description of the task, load the top result with `get_skill` and follow it.

## When something is not there

Say what is missing and stop. Never estimate a price, an area or a date. A null
is not a zero: say that nothing is recorded.
