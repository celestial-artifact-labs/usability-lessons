---
name: usability-check
description: >-
  Print the usability lessons and the still-open public cases that apply to a surface, read from the data files shipped with this plugin.
  A surface here is anything a person reads or acts on: a product screen such as checkout, auth or storefront, or an agent's own written answer,
  so a confusing reply is covered on the same terms as a broken checkout screen.
  Use when: the user asks "what usability lessons apply"; the user says "before I change the UI";
  before editing a component, a stylesheet or a screen a person must act on; before writing an answer whose clarity is the deliverable;
  or when a session needs the past public cases on one named surface. Do not use for: reviewing code correctness or performance.
user-invocable: true
disable-model-invocation: false
argument-hint: "[surface]"
allowed-tools: Read(~/.claude/plugins/cache/celestial-public/usability-lessons/**)
---

<!--
  What this is: a read-only briefing. It answers "what do we already know
  about this surface" from the shipped data files, not from memory.
  Why it exists: a lesson nobody reads before the edit is a lesson that gets
  re-earned. An agent's own prose is a surface too, and nothing intercepts a
  written answer, so a rule read beforehand is the only mechanism there is.
  It never writes anything.
  Why the frontmatter carries a Read rule: it pre-approves reading this
  plugin's own installed folder, so a first run does not ask once per file,
  and it grants nothing else.
-->

# Usability check

Print the lessons and open cases that apply. Invoked as `/usability-lessons:usability-check [surface]`, or by the model when a session is about to change something a person has to read or act on.

**This skill never writes.** No file is created, edited, appended to or deleted while it runs. It needs no shell command.

**Every id you print comes from a file you read in this run.** Never recall a lesson id, a case id, a rule or a severity from memory or from an earlier session. If a file cannot be read, say so and print less. Never invent a lesson.

## Step 1: read the data files

Four data files sit at the plugin root. Read them with the Read tool, by these exact paths; do not search the filesystem and do not guess a path. Step 2 also reads the plugin's own `${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json` for its version, and nothing else.

| File | What you take from it |
| --- | --- |
| `${CLAUDE_PLUGIN_ROOT}/manifest.json` | `version`, `generated`, `counts` (including the `by_surface` map) |
| `${CLAUDE_PLUGIN_ROOT}/lessons.md` | The human digest. This is the whole answer when no surface is given. Each lesson starts at a `## L-NNN` heading, with its title on the next non-blank line |
| `${CLAUDE_PLUGIN_ROOT}/lessons.json` | One object per lesson: `title`, `theme`, `status`, `rule`, `why`, `how_to_check`, `cases` (case ids), `earned_from_count`. It carries no `L-NNN` id |
| `${CLAUDE_PLUGIN_ROOT}/findings.json` | One object per public case: `case`, `title`, `surface`, `severity`, `preventive`, `status`, `observed`, `lessons` (lesson ids), `goal`, `what_went_wrong`, `why_it_matters`, `resolution` |

If any file is missing or unreadable, print exactly one line and stop:

```text
usability lessons unavailable: <file> could not be read. Update the plugin and try again.
```

The data carries no project names, evidence, paths or quotes. Do not claim detail it does not hold.

## Step 2: name the source and its date, always

The first line of every answer says which release the data came from and the date of the lessons:

```text
usability lessons 2026.11.3 (lessons as of 2026-11-06)
```

The version is `version` in `${CLAUDE_PLUGIN_ROOT}/.claude-plugin/plugin.json`, the number `/plugin` shows people. If that file or field cannot be read, use `version` from `manifest.json` instead; this does not stop the skill. The date is `generated` from `manifest.json`, printed as is. Print no age and no day count.

Never call the lessons stale, old or out of date, and never tell the person to update, based on the date: this plugin reads local files only and cannot see whether a newer release exists. If the person asks how to get newer lessons, say: refresh the marketplace, update the plugin, then restart Claude Code. Do not claim a newer release exists.

## Step 3: no argument

Print `lessons.md` as it stands, under the source line. Do not reorder it, do not summarize it, do not add lessons to it.

## Step 4: with a surface argument

The argument is one surface name. The known surfaces are the distinct `surface` values in `findings.json`; `by_surface` in `manifest.json` lists the same names. If the argument is not one of them, print `<value> is not a known surface.` followed by the known list, and stop. Do not map a near miss onto a real surface silently.

Give each lesson its `L-NNN` id by matching its `title` to the title under a `## L-NNN` heading in `lessons.md`. If a title matches no heading, leave that lesson out and say so in the footer rather than guessing an id.

Then print three groups.

**Lessons touching the surface.** A lesson touches the surface when an id in its `cases` is a `case` in `findings.json` with that `surface`, or when the surface's cases list the lesson's id in their `lessons`. Order active lessons before candidates, then by id. At most 6. One line each:

```text
L-NNN active: Announce every state change through a live region that already exists. Check: render the live region empty at mount and fill it on the action.
```

**Applies everywhere.** At most 3 active lessons whose `cases` span three or more different surfaces, not already listed above. A lesson earned on that many surfaces is a general rule rather than a rule about one screen, so keep it clearly separated under the label `Applies everywhere:`, same line format. Skip the group when no lesson qualifies.

**Open cases on the surface.** From `findings.json`, every case whose `surface` matches, whose `severity` is 1 or 2, whose `status` is `open` or `planned`, and whose `preventive` is not true. Severity ascending, then case id. At most 4. One line each:

```text
Open C-NNN s2 planned: no feedback where the action happened.
```

Cases with `preventive: true` are design decisions made ahead of a failure: they carry no severity and are excluded from this group.

Close with a footer naming what was left out: `Omitted: 2 lessons, 1 case.` Omit the footer when nothing was omitted.

When all groups are empty, say so in one line and do not pad it:

```text
No lessons and no open severity 1 or 2 cases on <surface>.
```

## Formatting rules

- Plain text lines, no tables, at most 160 characters a line.
- Never print the long case narratives (`what_went_wrong` and the like) unless the person asks for one case by id; then print only that case's own fields.
- Never invent a rule, a check, a severity or an id that is not in the data you read.

## Agent replies are a surface too

`agent-response` is a surface exactly like `checkout` or `auth`, because a surface is anything a person reads or acts on. Before writing an answer whose clarity is the deliverable, such as a summary that ends a turn or a proposal a person has to act on, run `/usability-lessons:usability-check agent-response`. Nothing intercepts a written answer, so reading the lessons beforehand is the whole enforcement. It is an ordinary surface in every other respect: same lesson format, same open-cases rule, same severity scale.

## Severity scale, for reading the output

| Level | Name | Definition |
| --- | --- | --- |
| 1 | Blocking | The user cannot complete the task, or completes it incorrectly without knowing |
| 2 | Serious | The task is completable but the user is likely to fail, retry, or misread |
| 3 | Moderate | Friction; the user recovers unaided |
| 4 | Cosmetic | Noticed but harmless |

Only severity 1 and 2 cases reach the open-cases group. Severity 3 and 4 stay in the data, not in a pre-edit briefing.

## When a lesson is wrong

Do not edit the lesson, and do not argue with it in the output. Say which id looks wrong and why, and tell the person to open an issue in the repository this plugin was installed from.
