# Dinacharya change log

**Comparison:** original attachment `index(9).html` → `Dinacharya-Updated.html`.
**Date:** 28 October 2026 (Australia/Melbourne).

The original attachment is the baseline available in this conversation. No earlier file literally named `index.html` was supplied, so this log does not claim a comparison to an unseen version.

| Area | Original | Updated |
|---|---|---|
| Learning structure | One shelf and AI builder on the same page | Three nested pages: My Vidya, My Vichara and Planner AI. |
| My Vidya categories | Software/Codex shared under one category | Skill, Topic, Codex and Software. |
| My Vichara categories | Interests, Vine and New growth labels | Hobby, Language/Vine and Topic to master. |
| Page headings | Collapsible shelf heading | Fixed page headings using the app font and transparent SVG line icons; only skills can hide. |
| Add learning button | Inline name/category form | +Add Skill/Hobby opens Planner AI; category defaults to the originating page. |
| Planner heading | Let AI shape your learning | Planner AI — Add your Skill/Hobby. |
| Planner levels | Beginner, Some experience, Confident with basics | Beginner; Somewhat / know terms; Intermediate; Refresh known skills; Expert / aiming for top 1%. |
| Time and duration | Minutes per session; up to 180 sessions | Hours per day; up to 365 training days; start date and daily/weekday cadence. |
| Planner context | Topic and outcome | Adds device/tools and notes for AI; includes daily capacity and scheduled load at the start date in the prompt. |
| Plan summary | Imported-session date range | Planned end date and total hours shown before import; actual plan start/end shown in skill trackers. |
| Planner workflow | Copy prompt, download template, upload or paste JSON | Copy prompt → paste AI JSON → preview → Add to My Vidya/My Vichara. Download/upload controls removed from the visible learning flow. |
| JSON persistence | Paste field lost during rerender | Pasted learning JSON retained in profile state until the tracker is created. |
| Tracker destination | Tracker embedded in shelf cards | All daily learning activities shown on My Vidya/My Vichara. Planner AI only handles setup/import. |
| Parallel learning | Independent skill plans | Multiple independent plans with a shared capacity budget and per-plan progress. |
| Capacity | No combined daily budget | Default 2 hours/day, editable to 0.25–24 hours; over-capacity imports and reschedules blocked. Existing overloaded dates display a warning. |
| Pause/resume | No controls | Pause excludes a plan from active workload. Resume schedules unfinished sessions from today using the original cadence. |
| Reschedule | Fixed imported dates | Reschedule unfinished sessions from a chosen date; completed dates and partial task completion are preserved. |
| Checkpoints | General prompt guidance | Prompt asks for a labelled Checkpoint every seven sessions and at the end; tracker shows matching sessions and deliverables. |
| Skill card fields | Practice plan/log, evidence and blockers fields | Those fields removed from display; existing saved data retained. Next action placed after Daily activity & checkpoints. |
| Resources | Free-text resource field | Resources moved down; title + validated HTTP/HTTPS link, add/remove controls and migration of the old resource text. Session guidance supports a resource title. |
| Milestones | Higher on each card | Moved below Resources; existing checklist behavior retained. |
| Activity title | Today's training | Activity. Movement counter heading changed to Movement minutes; walking/stretching description removed. |
| Activity restart | Start date changed without clearing completed workout days | Dedicated start date and confirmed restart clear workout completion/dates and missed-day shifts; return to Day 1 with session Undo. |
| Tracker start date | Earlier entries only became inaccessible | Confirmation now explains removal of earlier day-keyed entries and workout restart. Retained daily entries remain tied to their calendar dates. |
| Routine AI prompt | Generic daily schedule plus typed notes | Includes selected-date chores, care rotation, first-Friday mandir note, fixed activities and existing routine. Requests preserving fixed commitments and adding evening face care. |
| Learning navigation | Text labels/buttons | Transparent book, telescope and globe icons with accessible names/tooltips. |
| Shared navigation styling | Boxed/circular nested tabs with differing sizes | Main/nested navigation uses transparent backgrounds; nested icon buttons share 48px sizing and spacing; active selection uses a curved gold underline. |
| Animations | Primarily date transitions and hover styling | Page entrances, learning-card stagger/reveal, icon lift/glow and selection transitions across main/nested pages; reduced-motion preferences respected. |
| Chakra icon strip | Horizontally clipped checklist with hidden scrollbar | Actual visible checklist strip has left/right buttons, visible scrollbar and keyboard navigation; horizontal position survives rerenders. Selected activity gets a gold marker. |
| Home reflection panel | Taller quote panel | Slightly reduced minimum height and vertical padding; text remains free to expand. |
| Studio goal placement | Goals before Business Scratchpad, add form before tracked goals | Goals moved below Business Scratchpad; existing goals first and add-goal form last. |
| Goal cards | Full-width stacked cards | Compact responsive grid: two/three columns where space permits, one on phones; a lone card stays half-width on larger screens. |
| Pantry import bug — this revision | Object.keys interpreted array positions as item names | Recognizes arrays, keyed item records and legacy name maps; reads real item names and preserves quantities, units, thresholds and shopping completion. Bare numeric keys are not imported as names. |
| Numbered-entry recovery — this revision | Incorrect 0,1,2… entries displayed as pantry items | Conservative repair detects a sequential group of at least three numeric names with the legacy importer ID pattern. Suspected pantry/shopping entries are archived with original values, not permanently deleted. |
| Recovery controls — this revision | No repair history | Supplies/Pantry shows Import repair with per-entry Restore. Restored numeric names are protected from repeat cleanup; quantities remain recoverable. |
| Pantry item output — this revision | Names interpolated directly into HTML | Pantry names HTML-escaped for display. |

## Latest bug fix: what happens to existing data

- The HTML does not contain your private profile data. The repair runs when the app loads your saved state.
- Matching numbered entries move to `importRepairArchive`, preserving their original record, quantities and timestamp. Restore is available under Supplies/Pantry.
- Cleanup requires multiple signals. An isolated numeric name or an entry without the legacy importer ID pattern is left alone. Some ambiguous incorrect rows may therefore remain for review.
- Numeric labels alone cannot reveal a lost original item name. The corrected importer reads names from structured source records when those names still exist.
- Shopping imports receive the same shape handling so the bug cannot recreate numeric shopping names.

## Verification

- Complete HTML JavaScript syntax checked.
- Focused tests cover list imports, keyed records, legacy name maps, zero quantities, units, shopping completion and rejection of bare index keys.
- Repair tests cover archival, restoration, repeated runs and preservation of intentional numeric names.
- Earlier revisions were checked in browser preview for the real Chakra scroll control, transparent nested navigation and Goals ordering.
- This final pantry revision has automated checks; it has not been run against your private live profile or visually retested.

## Delivery scope

This request produces the updated HTML and this change log. The hosted Dinacharya site has not been republished with the pantry fix in this step.

The HTML still uses its existing external Firebase/CDN connections when online. Planner AI uses copy/paste JSON; it does not call an AI model automatically.

## Exact baseline identity

| File | SHA-256 |
|---|---|
| index(9).html | `5b67462c5b3c2b08ddbb965d31c2f593b1de208b52a4bf6c0c63bdf9c0824165` |
| Dinacharya-Updated.html | `bf7a467dff09f2e74a8283a976fe4d00e781afa7ae1154df070334a5a3cae3b6` |

Text comparison: 253 lines added and 100 lines removed. These counts include compact single-line HTML/CSS changes; they are not a feature count.
