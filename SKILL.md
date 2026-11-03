---
name: shutdown-ritual
description: Use when a user requests an end-of-work shutdown ritual, wants to close the workday and reclaim the evening, or calls it a "get off work funeral". Does not apply to general task estimation outside an end-of-day review.
---

# 🌙 Shutdown Ritual

Close today, plan tomorrow, and reclaim tonight through a short guided conversation and realistic time budgets.

## Interaction

- Ask **one question about one topic per turn**, then wait. Tool acceptance and checkbox drafts are not answers. Keep future questions internal.
- For free text or exclusive choices, use one question-tool item per call; without a tool, ask in chat. Never repeat the card's question in prose.
- Track the stage and reuse answers from this ritual. Advance after an answer or explicit skip. Shorten for fatigue; "finish now" means recap with gaps labeled.
- Be warm and gently playful; match language, aim for five minutes, and use occasional stage emojis. Keep labels and numbers readable.
- Keep plans in chat; saving or acting elsewhere requires a request.

## Sequence

| Stage | Ask or do next |
|---|---|
| ✨ Completed today | Review today's chats, then ask "What did you get done today?" Offer candidates with multiple selection; wait for confirmation. |
| 🧺 Unfinished | Ask only "What work is still unfinished?" Capture loose ends before planning. |
| 🌱 Tomorrow | Ask priorities, then each task's finish line and estimate in separate turns. Resolve uncertainty; establish available work time and a first step. |
| 🌙 Tonight | Establish free time after commitments, energy, activity choices, and durations through separate questions as needed. Rest counts; protect unplanned time. |
| Close | Once both periods have useful budgets, or closure is requested, return the tables and stop questioning. |

## Ground the opening in today's chats

Use the user's local date and timezone. With available history tools:

1. List active and archived chats, then read relevant user messages and final answers (`list_threads` / `read_thread`). Use metadata to shortlist; verify work from message/turn dates and outcomes. Renaming or archiving today does not date the work. Include and label overnight work completed today.
2. Retrieve short pages to today's boundary, omitting tool outputs where supported. Return compact excerpts, not logs or raw payloads. Merge related work, omit empty ritual tests, and retain supporting references.
3. Offer a short candidate list for confirmation, corrections, or additions. Discussion is "investigated," a plan is "outlined," and unfinished work remains unfinished. This chat alone is not a whole-day review.

Disclose partial coverage. If history is unavailable or yields no candidates, ask the ordinary free-text opening. Use other activity sources only when requested.

## Multiple selection

Accomplishments need checkboxes or real multi-select. Native selection requires schema support; single-choice `request_user_input_async` has no multi-select parameter. Do not substitute typing candidate numbers.

For inline rendering, follow the environment's guidance (visualize when available) and copy [assets/accomplishments.html](assets/accomplishments.html) to the task's visualization location. Populate `shutdown-accomplishments-data` JSON: local `date`, `timeZone`, and `items` with unique `id` and `label`. Escape `<` as `\u003c`. The chooser supports additions and "None of these."

Only **Continue** submits through `window.openai.sendFollowUpMessage`; wait for that message. Preserve selections on failure. Without native multi-select or inline rendering, explain briefly and confirm candidates individually with yes/no questions.

## Estimate and fit the plan

Clarify scope before estimating. Ask the user's initial estimate and comparable experience; probe uncertainty and confirm revisions. Include preparation, review, and transitions. Separate effort from external waiting. For unknown scope, a discovery timebox can be the next step while completion remains unknown.

Sum lower and upper estimates separately; add buffer once and exclude deferrals. Compare the upper total with available time. Plans fitting only the lower estimate are tentative. Resolve overflow with one choice to defer, reduce scope, or revise estimates; disclose unresolved overflow or assumptions in an immediate recap.

## Final output

Use confirmed answers. Return two populated Markdown tables with short emoji labels, then one closing sentence. Put caveats inside table cells.

| Table | Required columns and content |
|---|---|
| ✨ Your day & plans | Period · Task/result · Status/next step · Time estimate. Cover completed and unfinished work, deferrals, tomorrow's tasks and first step, and tonight's choices. |
| ⏳ Your time budget | Period · Task total · Buffer/free time · Planned/available · Spare or overflow. Include tomorrow and tonight, with ranges and uncertainty visible. |

Use "Unknown" for missing estimates and "Not recorded" for unmeasured past durations. Check arithmetic: tasks 50–70 min + buffer 20 min = 70–90 of 120 min, leaving 30–50 min.

Suggested close: "🌙 Shutdown complete. Your evening is yours." Then stop.

Inspired by [Cal Newport's shutdown ritual](https://calnewport.com/drastically-reduce-stress-with-a-work-shutdown-ritual/), extended with collaborative estimation and evening planning.
