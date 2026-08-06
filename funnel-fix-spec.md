# RX TRAINING FORM — Funnel Fix Spec

**Funnel ID:** `69a8ad795338da645a0bd459`
**Workspace:** `69a899b51e1622ca0038930d` (Jacob's Workspace)
**Live URL:** https://perspectivefunnel.co/69a899b51e1622ca0038930a/69a8ad795338da645a0bd459/

Context for a fresh session: the RX TRAINING FORM funnel was rebuilt around
recruiting on ~Aug 5 2026, on top of an older funnel aimed at adult recreational
runners. Leftover blocks from the old version are still in the flow. Diagnosis
below came from the CRM schema and contact records; **the step routing itself was
never readable** — the Perspective connector went down before it could be
inspected.

---

## 1. Delete leftover blocks from the old adult-runner funnel

| Field | Title | Why |
|---|---|---|
| `question_nmlo5t` | Tell us your age range | old funnel |
| `question_o999u4` | What have you already tried? | old funnel |
| `question_ngkgh1` | What does your schedule look like? | old funnel |
| `question_9fqabr` | Which parent or guardian helps? | old funnel |
| `question_ukuvyd` | How important is direct coach access during your training? | **duplicate** |
| `question_9gdrf9` | How important is direct access to your coach during your training? | **duplicate** |
| `eed5f8de-5533-4251-88b0-fb203ccca82e` | "Text" | stray untitled field |
| `input_h9rwfg` *or* `input_m7bp7t` | privacy policy checkbox | **two exist — keep one** |

The duplicate pairs are the most likely cause of misrouting.

---

## 2. Fix crossed field mappings

| Field | Currently titled | Problem | Fix |
|---|---|---|---|
| `birthday` (date type) | "Parents Phone number" | phone stored in a date field — will corrupt data | make it a real phone field |
| `address.city` | " Email Address" (leading space) | email in a city field; `email` already exists | delete |
| `lastName` | "Athlete Full Name" | full name crammed into last-name | split into athlete first + last |

Correct mappings already in place: `firstName` → Guardian First name,
`email` → Guardian email address, `phone` → Athlete phone number.

---

## 3. Add phone capture

Zero of the six August submissions captured a phone number. Every pre-August
contact has one. Add a required phone field on the contact step — the business
goal is booked calls, and email-only is a handicap.

---

## 4. Routing

Keep these six questions (currently working and consistently answered):

1. `question_h54knj` — What year do you/athlete graduate High school?
2. `question_lq2ev4` — What event does your athlete run? (multi-select)
3. `question_63rebf` — Where are you at with recruiting today?
4. `question_1gqchb` — How serious are you about competing in college?
5. `question_kkml6m` — What is the athlete's cumulative GPA?
6. `input_cfuxrv` — PRs / times (free text)

Branch on `question_63rebf`, the strongest intent signal:

- **Just getting started** → full question set → contact step
- **Speaking with a few coaches but nothing serious** → full set → contact step
- **I have offers/committed** → short path, skip GPA + events → contact step

**Verify every path terminates at a result page.** All six August submissions
have no `ps_completed_at` and no `ps_result_page`; every pre-Aug-5 submission has
both ("Thank you" + completion timestamp). Consistent with paths dead-ending —
but unconfirmed, since the step logic was never readable.

---

## 5. Add the booking link to the result page

> "Thanks — the fastest way to get your Match Report is 15 minutes on a call.
> Pick a time."

Booking link: Google Calendar appointment schedule (free, auto-attaches a Google
Meet link, handles timezones). This is the highest-intent moment in the funnel
and is currently empty.

---

## Priority

Do this before more ad spend. A paid IG test click dead-ended the same way as the
real leads.
