# #4471 · three problems, not one

owner: support desk · status: known · reviewed: 2026-10-28

[#4471](/kb/incidents/4471) was merged as one issue: "dates parse mm/dd in eu locales". Read one by one, its reports hold three different problems. Each needs its own ticket and its own fix.

> Not one issue. Three, with three fixes. Fixing the parser alone closes only the largest part.

## The split

**412** reports merged into #4471 · **3** problems

| problem | reports | tenants | seen |
|---|---|---|---|
| parser reads dates month first | 374 | 11 | 2026-04-01 → 2026-10-04 |
| booking-v3 form hint says MM/DD/YYYY | 33 | 3 | 2026-04-17 → 2026-09-22 |
| date moves by one day | 5 | 5 | 2026-04-02 → 2026-10-04 |

Tickets: issues #1 (374), #2 (33), #3 (5).

## The problems

1. **Parser reads dates month first.** Day-first tenants see day and month swapped, in every channel.
2. **Booking-v3 form hint.** The form prints MM/DD/YYYY under the date field; people in day-first countries type what it says. Form channel only.
3. **Dates move by one day.** Nothing is swapped. Likely a time-zone or daylight-saving bug.

## Why one ticket was wrong

Decision 0001 merges on subject alone, so reports with the same subject and different causes land together. These counts come from the report data, tenant locale and booking form.

## Lessons

- A big ticket can hide several bugs. Check the cause before you merge.
- Count each problem once, with the reports that belong to it.

desk ref **DR-0R3V-JYJ6**
