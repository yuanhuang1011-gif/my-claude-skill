# Decompose Check

Use this in Phase 1.3 to decide whether one request is really several stories.

## Split signals

Any one of these is enough to consider a split:

| Signal | Example |
|---|---|
| Two or more user goals | "Export the report **and** email it weekly" |
| Two or more personas with different needs | Ops wants speed, Finance wants an audit trail |
| Connector words joining separate asks | "and", "plus", "also", "while we're at it" |
| Parts that could ship independently and still be useful | A filter is useful even without saved views |
| Parts with very different effort or risk | A UI tweak bundled with a data migration |
| Parts owned by different teams or systems | Front-end change + a third-party integration |

**Not a split signal:** steps of one flow that are useless on their own (e.g.
"add a field" and "show the field" for the same goal).

## Breakdown table

When a signal fires, present:

| # | Story | User goal | Can ship alone? | Depends on | Rough size |
|---|---|---|---|---|---|
| 1 | … | … | Yes / No | — | S / M / L |
| 2 | … | … | Yes / No | #1 | S / M / L |

Then give a **recommended order** (usually: the smallest story that delivers
real value first) and the three options from SKILL.md Phase 1.3:

- **(a)** Ticket one story now, defer the rest
- **(b)** Proceed with all N stories
- **(c)** Keep as a single story (user states the reason)

## Dependencies

For each pair of stories, mark one of:

- **Blocks** — the second can't start (or can't be tested) until the first ships
- **Relates** — same area or customer, but independent

Default to **Relates** when unsure.
