---
name: withgrace
description: Use when working with With Grace property records, villas, projects, buyer contacts or ad campaigns. Reads and writes through the With Grace connector, which is scoped to the connected account's own records.
---

# With Grace

With Grace is an architecture and construction practice in Bali. This skill
connects an assistant to the With Grace connector so it can read and write the
records belonging to the connected account.

Setup is in [README.md](README.md). Once connected, the tools below are
available. Nothing here works without a connection, so if a tool is missing,
say so rather than guessing at an answer.

## What the records are

| Record | Holds |
| --- | --- |
| `organizations` | The company a set of records belongs to |
| `projects` | A development, with its name and its stage |
| `properties` | An individual villa or lot: its code, status, price, area and phase |
| `contacts` | People who have been in touch about a project |
| `ad_campaigns` | Marketing campaigns, and which finished cut each one uses |

Properties belong to a project. Projects belong to an organization.

**`properties` is the individual villa, not the development.** The two are easy
to swap and the mistake is expensive: answering about a whole development when
somebody asked about the villa they are buying. If a question is about price,
area, availability or phase, it is about a `property`. If it is about a stage,
a name or what is being marketed, it is about a `project`.

Call `list_entities` if you are unsure what exists. It is the authority; this
table is a description of it and can fall behind.

## Tools

| Tool | Use |
| --- | --- |
| `list_entities` | The record kinds available |
| `count_records` | How many of each kind exist |
| `list_records` | Read records of one kind, with an optional `limit` |
| `get_record` | Read one record by its id |
| `create_record` | Add one record |
| `update_record` | Change fields on one record |
| `project_overview` | Where each project is, what is true now, and what happens next |
| `set_project_stage` | Move a project to land, design, development or complete |
| `start_project_marketing` | Begin marketing a project. Refused while it is still on land |
| `latest_ad` | The newest finished ad for a project, and where to fetch it |
| `list_connectable_projects` | Which projects could have a CRM connected |
| `crm_connection_status` | What one project has connected, and what needs reconnecting |

Your client's own tool list is the authority, not this table. A connection may
expose more than this skill describes, including tools for records your account
cannot read; those refuse in words rather than returning nothing, so report the
refusal rather than treating it as an empty answer.

## How to use it well

**Every figure comes from a record.** Price, area, availability, phase and any
campaign number live in the rows these tools return. Never estimate one, never
average one into existence, and never carry a figure from an earlier answer.
If a record is missing the field, say the field is not recorded.

**Read before writing.** `list_records` or `get_record` first, so an update
changes what you think it changes. `update_record` reports which fields it
ignored; read that rather than assuming a write landed.

**The connection decides what is visible.** These tools return the connected
account's own records and nothing else. An empty result means there is nothing
there for this account, not that the tools failed. A refusal says so in words:
if you get one, report it rather than treating it as an empty answer.

**Some fields cannot be changed.** A record's id, its creation time and the
organization it belongs to are fixed. Attempting them is ignored rather than
refused, which is why the ignored list is worth reading.

**Ask before creating.** A created record is real to everyone who reads it
afterwards. Confirm the details with the person first.

## Worked example

> Which villas are still available at the hillside project, and what do they cost?

1. `list_records` with `entity: "projects"` to find the project and its id
2. `list_records` with `entity: "properties"`
3. Filter to that project's id, and to `status: "available"`
4. Report the codes, prices and areas exactly as the records give them

If none is available, say so. Do not offer the nearest alternative as though it
were available.

## When something is not there

Say what is missing and stop. "No price is recorded for that villa" is a useful
answer. An invented figure about a property someone may buy is not, and it is
the failure this skill exists to prevent.
