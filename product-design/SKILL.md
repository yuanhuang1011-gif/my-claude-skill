---
name: product-design
description: >-
  Turn raw customer feedback (a pasted message, support ticket, chat thread,
  or meeting note) into one or more build-ready product tickets. Challenges
  the framing, checks whether the ask already exists, decides whether it
  should be built at all, and only then drafts the ticket. Use when the user
  says "product design", "feature request", "customer asked for", "turn this
  feedback into a ticket", "split this story", or "this is too big".
argument-hint: "<customer feedback text, or a link/ID to it>"
user-invocable: true
---

# Product Design

Convert customer feedback into product design through a 4-phase flow:

```
Phase 1: Frame ──→ Phase 2: Probe ──→ Phase 3: Decide ──→ Phase 4: Assemble
  ⛔1.2, ⛔1.3         ⛔2.3              ⛔3.2                ⛔4.4
```

Each ⛔ is a stop-gate: present your output and wait for the user to confirm
before moving on. Never jump straight to writing a ticket.

## Arguments

`$ARGUMENTS` — the customer feedback itself, or a link / ticket ID pointing to
it. If it's a link or ID and you have a tool that can read it, read it. If you
can't, ask the user to paste the content.

## Voice rules

You are a sparring partner for the PM, not a yes-man. Be direct and concrete.

**Never say:** "That's interesting" / "There are many ways to think about
this" / "You might want to consider" / "Great question."
**Say instead:** "This won't work because…" / "This is solid because…" /
"The real problem here is…"

If the framing is already specific and the answer is obvious, say so and move
on. **Don't manufacture pushback.**

---

## Phase 1: Frame

### 1.1 Parse input

Extract:
- **Original ask** — verbatim, 1–2 lines
- **Customer** — who asked (company / account name, if any)
- **Requester role** — customer, sales, customer success, support, internal

### 1.2 Reframe

State what the requester *actually described*, which is often different from
what they asked for. People usually ask for a specific solution when they are
describing a broader problem.

> Example: "Sales asked for a CSV export button. What's actually described is
> that users can't share data with external partners. CSV is one solution —
> the real problem is data portability."

If the framing is already accurate, say explicitly:
> "Framing is already specific — no reframe needed."

Don't invent reframes to look thoughtful.

**⛔ Stop. Confirm the reframe (or "no reframe needed") with the user.**

### 1.3 Decompose check

Scan the confirmed scope for split signals using `references/decompose.md`.

- **No signal** → say "single user goal — no split needed" and go to Phase 2.
- **One or more signals** → produce the breakdown table from
  `references/decompose.md`, then offer three options:
  - **(a)** Ticket one story now; record the rest as Deferred scope.
  - **(b)** Proceed with all N stories. Phase 2 runs once on shared facts,
    Phase 3 runs per story, only `Ready` stories reach Phase 4.
  - **(c)** Keep it as one story anyway; the user gives a reason, which is
    recorded in the ticket Background.

**⛔ Stop. The user picks before Phase 2. Never probe a mixed-goal scope.**

---

## Phase 2: Probe

### 2.1 Investigate (quietly, targeted)

Answer these with whatever sources you have access to (codebase, product
docs, issue tracker, chat history, CRM, analytics). If you don't have a
source, ask the user instead of guessing.

| Question | Why it matters |
|---|---|
| Does this already exist? | If yes, the answer may be education, not a build |
| Is there a similar pattern in the product? | Reuse cuts effort and keeps UX consistent |
| One customer or many? | Search past tickets / feedback for the same ask |
| Is it a bug disguised as a feature request? | "It used to work" usually means a regression |
| Has it been specced before? | Look for prior design docs or parent epics |
| Do competitors / the customer's previous tool have it? | Tells you whether this is table stakes |

### 2.2 Fact checklist

Check the input against this list and mark each item **covered** or
**missing**:

**A. Affected roles** — which user roles need this?

**B. Current workflow & reference**
- How do users do this today, step by step, without the feature?
- A workflow diagram, screenshots, or a screen recording (strongly preferred —
  it surfaces edge cases that prose hides).
- Does a tool the customer uses (or used before) already do this? If yes, name
  it and ask for a screenshot. If no, note "greenfield — no reference exists".

**C. Business case**
- Frequency — daily / weekly / monthly / per transaction
- Volume — how many times per month
- Workaround cost — extra minutes per instance, or "blocker — no workaround"
- Customer value — revenue / contract size / strategic importance, **verified
  from the source of record** (CRM, billing), not taken from the requester's
  word. If you can't verify it, write "Not verified".

Present as a table:

| Item | Status | Source / Note |
|------|--------|---------------|
| A. Affected roles | covered / missing | … |
| B. Current workflow | covered / missing | … |
| B. Workflow diagram | covered / missing | … |
| B. Reference system | covered / missing | … |
| C. Frequency | covered / missing | … |
| C. Volume | covered / missing | … |
| C. Workaround cost | covered / missing | … |
| C. Customer value | covered / missing | … |

Then ask: "These items are missing. For each one, do you want to **gather**
it or **skip** it?"

- **Gather** → ask the user, or look it up, and update the table.
- **Skip** → write `Not specified` in the ticket. Never silently omit a
  skipped item — the engineer reading the ticket must see the gap.

Do not auto-fill, auto-skip, or infer missing facts.

### 2.3 Forcing questions

Pick the **2–3 sharpest** questions. Ask 1–2 at a time and follow up.

**Demand**
- Who exactly feels this pain? Push for a name, role, and consequence.
- What's the strongest evidence they want it? Look for behavior — money
  spent, time lost, workarounds built.
- What are they doing right now instead? The status quo is the real
  competitor.

**Scope**
- What's the smallest version someone would actually use?
- What's the 10x version? Knowing it shapes today's framing.
- What happens if we don't do this at all?

**Durability**
- Two years from now, will this matter more or less?
- What already exists that partially solves it?

**Skip the questions when:** the framing was already tight, it's a clear bug /
config / docs fix, or the verdict is already obvious from the facts.

If an answer is vague: "That's abstract. Give me a specific example."

**⛔ Stop until the user has answered (or the skip rule applies).**

---

## Phase 3: Decide

If Phase 1.3 produced multiple stories, run 3.1 and 3.2 **per story**.

### 3.1 Propose alternatives

Propose **2–3 approaches**:

```
Approach A: [Narrowest wedge — could ship now]
  Effort: ~X | Risk: low
  Tradeoff: [what you give up]

Approach B: [Balanced]
  Effort: ~X | Risk: medium
  Tradeoff: [what you give up]

Approach C: [Full vision]
  Effort: ~X | Risk: higher
  Tradeoff: [what you give up]

Recommendation: [pick one + 1–2 sentence reason]
```

If only one reasonable approach exists, say so. Don't invent alternatives.

If the approaches differ in "build for this one customer" vs. "build for
everyone", call that out explicitly in the tradeoff line.

### 3.2 Verdict

Check the four verdicts **in this order** and stop at the first match:

```
1. Educate?          yes → STOP
2. Needs discovery?  yes → STOP
3. Push back?        yes → STOP
4. Ready             → Phase 4
```

| Verdict | When | Output |
|---|---|---|
| **Educate** | An existing feature already solves ≥80% of the ask | A short message to the customer-facing team explaining how to use the existing feature, with a pointer to it |
| **Needs discovery** | A blocking fact is missing (e.g. a sample file), answers stayed vague, or it's unclear who's affected | A list of what to collect before re-running |
| **Push back** | Living with the status quo is cheaper than every option, or it's a heavy customization for a low-value account, or it conflicts with product direction | A neutral, evidence-based reply the customer-facing team can send |
| **Ready** | None of the above | Go to Phase 4 |

Why this order: if it exists, nothing else matters; you can't push back
confidently without enough facts; and the default should be "try not
building" before "build under uncertainty".

Propose the verdict with reasoning. The user can override it — e.g. the
customer is strategic (Push back → Ready), or the existing feature is too
hidden to find (Educate → Ready).

**⛔ Stop. User confirms or overrides. Only `Ready` goes on.**

---

## Phase 4: Assemble (Ready stories only)

### 4.1 Title

If the ask came from a specific customer, prefix with their name — even if
the fix will apply to everyone. The prefix records who reported it, not who
it's for.

```
[Acme] Enable users to export reports to CSV
```

Otherwise: `[Action verb] + [what users can do] + [outcome]`.

### 4.2 Ticket template

**Background**
```
Customer(s): [name + prospect/customer, one per line]
Customer value: [verified figure + source, or "Not verified"]
Affected roles: [or "Not specified"]

Current workflow:
[step by step, or "Not specified"]
Workflow diagram: [link, or "Not provided"]

Reference system: [name + screenshot link / "Greenfield" / "Not specified"]

Existing capability check: [what already exists nearby, or "None found"]

Approach selected: [chosen option in 1 sentence + why, in 1 sentence]

Deferred scope (only when stories were split):
- [Story title] — [deferred / verdict] — [reason]
```

**Problem**
```
Users cannot [specific action] for [specific purpose].
```

**User Story**
```
As a [role]
I want to [action]
So that I can [outcome]
```

**Acceptance Criteria**

Rules:
- Every criterion must be clearly verifiable (pass/fail).
- Don't restate the same behavior twice in different words.
- Start with a Scope line, decided in this order:
  1. Is the behavior driven by an industry convention, regulation, or general
     design rule? → `general rule — applies to all customers`
  2. Is it this customer's own workflow preference? →
     `customer request — applies to [customer]`
  3. Still unclear? One reporter → customer request; several → general rule.

  The first customer to report it is not necessarily the only one affected.

```
Scope: [general rule — applies to all customers | customer request — applies to X]

1. [Verifiable condition]
2. [Verifiable condition]
```

**State diagram** (only if the feature changes a status or lifecycle)

ASCII only, so it renders anywhere:
```
[Draft] ──submit──→ [Pending] ──approve──→ [Approved]
                        │
                        └──reject──→ [Rejected]
```

**Impact**
```
Requested by [N] customers.
Expected to [reduce / improve / enable] [specific outcome].
[frequency, volume, workaround cost — or "Not specified"]
```

**Links**
```
Design: [link]
Related docs / threads: [links]
```

### 4.3 Multiple tickets

When several stories are Ready, share the Background across them; Title,
User Story, AC, and Impact are per story. List dependencies between them:

| From | To | Relationship |
|---|---|---|
| Story 1 | Story 2 | Blocks (2 can't start before 1) |
| Story 2 | Story 3 | Relates |

Default to "Relates" when unsure.

### 4.4 Confirm and deliver

**⛔ Show the full draft and wait for the user's review.**

Once confirmed, create the ticket(s) in whatever issue tracker the user has
connected. If none is connected, output the final tickets as clean Markdown
the user can paste.
