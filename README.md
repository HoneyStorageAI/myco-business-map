# The Myco Business Map

A map of what an owner-led business actually runs — built so we can colour in which
parts Myco covers and which parts the owner is still doing by hand.

Live: https://anton-zaides-hb.github.io/myco-business-map/

Everything lives in a single self-contained `index.html`. No build step, no
dependencies, no CDN. Open it by double-clicking.

---

## The model

The map has two halves, and one test decides which half a piece of work belongs to:

> **Does this exist because of a specific client?**
> Yes → **Client-based.** No → **Support-based.**

**Client-based** runs as a flow — one pass per client, left to right.
**Support-based** has no sequence. Those areas run continuously whether or not
a single client exists.

This split is the whole point. A customer-journey map alone drops payroll, insurance
and equipment on the floor. A function-based map alone loses the sense of a client
moving through stages. Two halves keeps both without forcing one into the other.

Inspired loosely by E-Myth (every hat the owner wears) and EOS (a small business has
only a handful of core processes), but deliberately cut for owner-led businesses
rather than companies with departments. Enterprise frameworks — APQC's process
taxonomy, Porter's value chain, TOGAF capability maps — were considered and rejected
as far too heavy for this audience. Don't reach for them.

---

## The three levels

Each level is a tab.

| Level | Tab | What it is |
|---|---|---|
| 1 | **General** | Any business with clients. The shared spine. |
| 2 | **Segment** | One industry — e.g. Venues. Same two halves, different steps. |
| 3 | **Business** | One real company, as it actually operates today. |

Level 2 is free to restructure level 1, not just relabel it. Venues, for example:

- Added **Tour** as its own stage — for a venue it's where the booking is won or lost.
- Split **Deliver** into **Plan** and **Event day**, because a venue delivers over
  12–24 months and then one intense day.
- Replaced the **Tools & assets** support area with **The venue**.
- Added a whole new support area, **Vendor network**, which no generic business has.

If a segment doesn't restructure anything, it probably didn't need its own tab.

---

## States (level 3 only)

Level 3 boxes carry a `data-state` that colours them. This is what turns the map from
a description into an argument about where to start.

| State | Colour | Meaning |
|---|---|---|
| *(none)* | normal | Live today. Happening, generally by hand. |
| `focus` | orange | **What we build first.** Should map to the milestones actually pitched to the client. |
| `dormant` | grey | Paused by circumstance, not by choice. Can't be tested right now. |
| `out` | dashed | Deliberately out of scope, with the reason recorded in the note. |

Orange is not "every gap" — it's the agreed first build. Keep it tight enough that it
reads as a plan. A real gap we can't work on yet stays normal or dormant.

---

## Every box carries questions

Each box opens a drawer with two things:

- `data-note` — one or two sentences: what this is, and what usually goes wrong.
- `data-questions` — a `|`-separated list of questions to ask the owner.

**Questions, not "ask the client".** An early draft was full of notes saying "not
confirmed — ask Jose", which is useless in a meeting. Replaced with real question
lists, so the map doubles as the agenda. Orange boxes get deep lists (8–13 questions,
covering exact rules, where the data lives, thresholds, who approves, who gets told).
Other boxes get 2–4 lighter ones about how things work today.

The questions that matter most, and that keep getting missed:

- **Where does the data physically live?** HoneyBook, a spreadsheet, or someone's head.
- **What are the exact thresholds?** "Too long without a reply" has to become a number.
- **Who approves, and who gets told?** Owner, manager, or nobody.
- **What would make you actually use this weekly?** The difference between a feature
  and something that gets ignored.

---

## Sourcing rule

**Only assert what's in the source material. Everything else becomes a question.**

Level 3 describes a real business and real named people. Guessing how they operate and
writing it as fact is worse than leaving a gap, because nobody can tell the difference
later. Where we don't know, the note says so plainly and the questions carry the weight.

Source material for a level 3 tab comes from that client's bruno project:

```
projects/{project-key}/exploration/client-brief.md
projects/{project-key}/discussions/*-kickoff-call-context.md
projects/{project-key}/discussions/*-milestone-roadmap.md
```

The milestone roadmap is what decides which boxes are orange.

---

## Adding a tab

1. Add a button to `.tabs`:
   ```html
   <button class="tab" data-panel="my-panel">My Business</button>
   ```

2. Add the panel. Client row and support row, each a `.row` whose `--n` is the number
   of columns:
   ```html
   <div class="panel" id="my-panel">
     <section>
       <div class="sec-head client-scope">
         <span class="pip"></span><h2>Client-based</h2>
         <span class="note">One line of context</span><span class="line"></span>
       </div>
       <div class="row client" style="--n:7">
         <div class="col">
           <div class="head" data-note="..." data-questions="Q1|Q2">Stage</div>
           <div class="sub" data-note="..." data-questions="Q1|Q2">Task</div>
         </div>
       </div>
     </section>
     <!-- repeat with support-scope / .row.support -->
   </div>
   ```

3. For level 3, add `data-state` and the matching class together:
   ```html
   <div class="sub focus" data-state="focus" data-note="..." data-questions="...">Task</div>
   ```

4. Level 3 panels should open with the `.legend` block so the colours are explained.

The arrows between stages, the drawer, and the tab switching are all automatic. No JS
changes needed to add a tab.

---

## Writing rules

Learned the hard way. Breaking these is what made earlier drafts unusable.

- **Plain, universal words.** "Rota" is British and meant nothing to the reader —
  it became "staff scheduling". Avoid regional or industry jargon.
- **Concrete, never vague.** "The space itself" told nobody anything; "premises and
  utilities" does. If a box title could mean three things, rewrite it.
- **Box names should carry weight.** "Planning" was flat and became
  "Growth & strategy". The label is doing real work on a crowded screen.
- **Uneven counts are correct.** Columns do not need the same number of boxes.
  Padding a column to match its neighbour adds fluff.
- **No framework name-dropping in the UI.** Keep E-Myth, EOS and the rest in this
  README. The map itself should read as ours.
- **No intro prose.** Title, tabs, map. Anything explanatory belongs in a box or here.
- **Outcomes are not tasks.** "Win repeat work" and "keeping good people" were cut —
  they're results, not work anyone does on a Tuesday.

---

## Decisions on record

- **Marketing sits in Awareness (client-based), not support.** Arguable, since an
  Instagram post isn't attached to anyone yet. The call: the client flow starts when a
  real person appears, and marketing is what makes them appear, so it leads the flow.
- **"Guests as future leads" is out of scope.** Wedding guests never consented to be
  contacted. Messaging them risks the sending number being blocked and damages the
  client's name. Revisit only as a genuine opt-in offered at the event itself.
- **No "happens at any stage" band.** An earlier draft had cancellations and
  complaints in a band running under the flow. It was cut for being visual clutter;
  those items now live in the stage where they most often land.
