---
name: shutdown-ritual
description: Use when a user requests an end-of-work shutdown ritual, wants to close the workday and reclaim the evening, or calls it a "get off work funeral". Does not apply to general task estimation outside an end-of-day review.
---

# Shutdown Ritual

Help the user close today's work, give tomorrow a starting point, and choose an evening that fits their time and energy. Finish with a realistic time budget for both work and personal activities.

## Conversation

Be direct, warm, and curious. Reuse answers already given and aim for roughly five minutes. Probe vague plans without guilt or interrogation. Match the user's language.

### Question cards

Each turn contains a brief reflection when useful and **exactly one current question**, or the final recap. A question requests one answer about one topic. Keep future stages internal; do not display a questionnaire or combine completed work, unfinished work, priorities, and estimates in one prompt.

For free-text questions and mutually exclusive choices, use `request_user_input_async` or an equivalent structured question tool when available. Make one call containing exactly one question item, then end the turn. Wait for the user's actual reply before asking the next question. An asynchronous tool's acceptance is not an answer. While a question is unanswered, keep it pending rather than issuing another question or advancing stages.

Offer a few suggested answers when they help the user make a choice, using their known tasks and constraints. Keep free-text answers available. For task descriptions or time estimates, omit suggested answers when none are grounded in the conversation. Put the question in the card only; use chat for a brief reflection or explanation. If no question tool is available, ask the same single question in chat.

### Before the first question: review today's chats

When chat-history tools are available, review accessible Codex conversations for the user's current local day before asking about completed work. Use the user's date and timezone; do not substitute the execution host's timezone.

- Use `list_threads` or an equivalent to find recent active and archived chats. Titles, summaries, and modification times are leads, not evidence of completion. Read relevant messages with `read_thread`, using short pages and `includeOutputs: false` where supported. Check message or turn timestamps for work occurring today; an older chat archived or renamed today does not establish work done today. Include completion times for work that started before midnight and finished today; label overnight items when useful so the user can decide whether to count them. Paginate when needed to reach today's messages, then stop at the day boundary.
- Extract a short list of distinct, substantive candidate accomplishments from user messages and relevant final answers. Keep the source chat and today's supporting message available for verification. Merge related discussions and omit empty ritual tests. Preserve the observed outcome: discussing a fix is "investigated," planning is "outlined," and an unfinished implementation stays unfinished. Every suggestion still needs the user's confirmation.
- Keep retrieval compact: return relevant excerpts or outcome notes rather than entire chat payloads, command logs, or older unrelated turns. Say briefly if the available history covers only part of the day. If history is unavailable or yields no useful candidates, proceed with the ordinary free-text opening question.

Use these candidates in the first "What did you get done today?" question. Completed items are independent: the user must be able to select several directly, using checkboxes or a native multi-select. Allow corrections and additions. Keep this as one question about completed work, then wait. Suggestions do not count as confirmed answers and do not permit advancing the ritual automatically.

### Multiple selection

Use a native multi-select only when its actual tool schema supports it. The single-choice `request_user_input_async` schema has no multi-select parameter; do not invent one or ask the user to type candidate numbers as the standard interaction.

When inline interactive rendering is available, use [assets/accomplishments.html](assets/accomplishments.html) for the opening question. Follow the environment's rendering guidance, such as the visualize skill when available. Copy the fragment to the task's visualization location and populate only its `shutdown-accomplishments-data` JSON with the current local `date`, `timeZone`, and verified `items` (`id`, `label`). Give items unique IDs and serialize JSON safely, escaping `<` as `\u003c`. The template renders labels as text, permits several checkboxes, accepts additions, and offers "None of these."

Selection changes are drafts. Only **Continue** submits all choices and additions to chat with `window.openai.sendFollowUpMessage`; do not advance based on saved display state. Wait for that explicit submitted answer before asking about unfinished work. If the form cannot submit, retain the selections and acknowledge the limitation instead of treating them as confirmed.

If neither native multi-select nor interactive rendering is available, briefly explain the limitation and confirm candidates one by one with yes/no questions, collecting multiple accepted items across turns. Keep the remaining ritual one question at a time.

### Sequence

Track the current stage and known answers in the conversation. Advance after the user answers, explicitly skips, or has already supplied the answer during this ritual. Today's accessible chat history is the default input for opening suggestions; use other activity sources only when requested. A task completed in this chat does not substitute for asking about the user's whole day.

1. **Completed today.** After the history review, ask only about completed work, offering grounded candidates when available. Wait for the user's confirmation, corrections, or additions before discussing unfinished work.
2. **Left unfinished.** Ask only: "What work is still unfinished?" Capture the loose ends, then wait before planning.
3. **Tomorrow.** First ask what they want to prioritize tomorrow. After their reply, clarify one selected task's finish line. In a later turn, ask for its time estimate. Follow up on a specific uncertainty only when needed; confirm a proposed revision before treating it as agreed. Repeat for other selected tasks. Ask about tomorrow's available work time separately if unknown.
4. **Tonight.** First ask about available evening time if unknown. Ask about energy, activity choice, and activity duration in separate later turns as needed. Rest is a valid choice. Keep deliberate unplanned time. If the plan is crowded, ask the user to choose a deferral, scope reduction, or revised estimate.
5. **Close.** Produce the recap once both periods have enough information for a useful time budget, or the user requests it. A request to "finish now" ends questioning; summarize known facts and label missing estimates or assumptions.

If the user requests a shorter ritual or is tired, reduce follow-ups while keeping one question per turn. Mark skipped details as unknown.

### Estimation and capacity

Use the user's initial estimate and evidence, such as similar past work. Resolve vague scope before accepting a completion estimate; a discovery timebox may be the next step while completion remains unknown. Include preparation, review, and transitions when relevant. Separate active effort from external waiting time, which may prevent promising a finish time.

Sum lower and upper task estimates separately and add explicit buffer once. Compare the upper total with available time. Plans fitting only the lower total are tentative. Resolve overflow through one focused choice before presenting a committed plan; if the user requests an immediate recap, identify the overflow clearly.

## Final recap

Show today's progress, tomorrow's tasks and first step, tonight's choices, and deferrals. For each period, show task durations, total range, buffer or protected free time, available time, and remaining capacity. Keep uncertain estimates visible. End with a closure phrase and stop prompting for more work.

Example with all inputs already supplied:

> Closed today: report draft.
>
> Tomorrow: revise 40–60 min; send 10 min. Tasks 50–70 + buffer 20 = **70–90 of 120 min**; spare 30–50. Defer the numbers check. First step: open the draft and mark revisions.
>
> Tonight: walk 20 min; read 15–25 min. Activities 35–45 + protected free time 15 = **50–60 of 60 min**; spare 0–10.
>
> Shutdown complete. Your evening is yours.

Keep the plan in chat; saving it or acting in other apps requires a request.

Adapts [Cal Newport's shutdown ritual](https://calnewport.com/drastically-reduce-stress-with-a-work-shutdown-ritual/) with collaborative estimation and evening planning.
