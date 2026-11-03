# Shutdown Ritual

A Codex skill for closing the workday and making the evening your own.

Through a short guided conversation, it captures today's progress, clarifies unfinished work, and helps estimate tomorrow's tasks and tonight's activities. The final recap shows realistic duration ranges, buffer, available time, and what to defer. Rest is a valid evening plan.

## Use

Once installed, invoke the skill in a new chat:

```text
Use $shutdown-ritual to help me close my workday.
```

The ritual starts with "What did you get done today?" and waits for your answer. It then moves through unfinished work, tomorrow's plan, and tonight's plan. It asks exactly one question per turn, including separate questions for task scope and time estimates. Question cards offer suggested choices when useful and room for your own answers; environments without cards use chat.

## Install

Place this repository folder at `$CODEX_HOME/skills/shutdown-ritual`. When `CODEX_HOME` is unset, the default location is `~/.codex/skills/shutdown-ritual`.

If you already have this skill installed, review your existing copy before replacing it.

## Files

- [SKILL.md](SKILL.md) contains the skill instructions.
- [agents/openai.yaml](agents/openai.yaml) contains the display name and invocation prompt.

## Inspiration

Inspired by [Cal Newport's shutdown ritual](https://calnewport.com/drastically-reduce-stress-with-a-work-shutdown-ritual/), with collaborative time estimation and evening planning.
