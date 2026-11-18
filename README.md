# usability-lessons

Usability lessons learned across products, published in project-free form: the rule, why it holds, how to check it.

## What this repository is

This repository publishes general usability lessons, written for people who design, build or review interfaces and want rules they can check their own work against. It publishes two kinds of record:

- **Lessons.** A lesson has three parts:
  - **The rule.** A general statement of what to do, or not do, so that people can use what you build.
  - **Why it holds.** The reason the rule is true, so you can judge whether it applies to your situation.
  - **How to check it.** A step someone else can repeat to find out whether the rule was followed.
- **Public cases.** A public case is a short account of one usability failure, or of one decision taken to prevent a failure: what a person was trying to do, what went wrong, why it mattered and how it ended. Each lesson lists the public cases it was learned from, where there are any.

"Project-free" means that no lesson or case names the project, product, company, repository, person, brand, screen name or key binding it was learned from. What is published is the general rule, not the story of where it came from. Both kinds of record are exported from a private ledger of usability findings, and every record passes a mechanical leak check before it is exported. The export writes nothing at all if any record fails the check.

## Status

Releases are published as dated snapshots; see [CHANGELOG.md](CHANGELOG.md) for the releases so far. This repository contains data and documentation, plus an optional Claude Code plugin that reads that data (see "Using the Claude Code plugin" below). There is nothing to build or run. Some lessons list no public case yet, and the set of lessons and cases will keep growing.

## Using the Claude Code plugin

The same repository is also a Claude Code plugin marketplace, `celestial-public`, with one plugin, `usability-lessons`. The plugin prints these lessons, and the still-open serious public cases for a surface, so you can read them before you change an interface. It is optional: the data files work without it.

Install it by typing these two commands inside Claude Code. They are slash commands, not shell commands:

```text
/plugin marketplace add celestial-artifact-labs/usability-lessons
/plugin install usability-lessons@celestial-public
```

The same two steps from a shell:

```bash
claude plugin marketplace add celestial-artifact-labs/usability-lessons
claude plugin install usability-lessons@celestial-public
```

Use it by typing these inside Claude Code:

```text
/usability-lessons:usability-check
/usability-lessons:usability-check checkout
```

With no argument it prints all lessons. With a surface, such as `checkout`, `auth` or `agent-response`, it prints the lessons for that surface and its open cases of severity 1 or 2. Claude may also run it on its own before it changes something a person has to read or act on.

Update it by hand, because auto-update is off by default for third-party marketplaces. Each release carries a new plugin version. Refreshing the marketplace listing does not by itself update the installed plugin, so it takes three steps: refresh the listing, update the plugin, then restart Claude Code.

Inside Claude Code, type this to refresh the listing:

```text
/plugin marketplace update celestial-public
```

Then open `/plugin` and update `usability-lessons` from the installed plugins there. Then restart Claude Code.

From a shell, refresh the listing:

```bash
claude plugin marketplace update celestial-public
```

Then update the plugin:

```bash
claude plugin update usability-lessons@celestial-public
```

Then restart Claude Code.

The plugin only reads the data files in this repository. It writes nothing, has no hooks and sends nothing anywhere.

The plugin pre-approves reading its own installed folder, `~/.claude/plugins/cache/celestial-public/usability-lessons/`, so Claude Code does not ask once per file, and it pre-approves nothing else. If Claude Code keeps its files somewhere else, or your organization allows only its own managed permission rules, it may still ask before reading the plugin's own installed copies of these data files.

## Files

| File | What it is |
| --- | --- |
| [lessons.md](lessons.md) | Every lesson as one readable document. Start here if you are a person. |
| [lessons.json](lessons.json) | The same lessons as data. |
| [findings.json](findings.json) | The public cases as data, one row per case. |
| [manifest.json](manifest.json) | When the snapshot was generated, and how many cases and lessons it holds. |
| [CHANGELOG.md](CHANGELOG.md) | What changed in each release, by lesson id. |
| [LICENSE](LICENSE) | CC0 1.0 Universal. |
| `.claude-plugin/` and `skills/` | The Claude Code plugin and its marketplace listing, written by the release tooling. See the section above. |

`lessons.md`, `lessons.json`, `findings.json`, `manifest.json`, `CHANGELOG.md`, `.claude-plugin/` and `skills/` are generated by the release tooling and are never edited by hand here. Each release overwrites `.claude-plugin/` and `skills/`, so a hand edit to either would be lost. This README and `LICENSE` are maintained by hand.

## Versions and the changelog

Releases are numbered with Calver, `YYYY.MM.N`. The year and month come from the `generated` date in `manifest.json`. `N` is the release's sequence number within that month: it starts at 1 and rises only when a second release ships in the same month. Because the month is the snapshot's own, it can trail the day a release is published. It is never the day a lesson was written.

Each release is one commit, `docs(lessons): release YYYY.MM.N`, and one tag, `lessons-vYYYY.MM.N`. The tag is how you refer to a release.

`manifest.json` also has a field called `version`, and the header of `lessons.md` repeats it. That value is a date stamp of the snapshot in the form `YYYY.MM.DD`. It is not the release version. Identify a release by its tag or its changelog heading.

`CHANGELOG.md` lists releases newest first. Each entry is headed `YYYY.MM.N - YYYY-MM-DD` and is built by comparing that release's `lessons.json` with the previous release's, lesson by lesson, using the lesson id (`L-NNN`, which is stable and never reused or renumbered):

- **Added** lists ids that were not in the previous release.
- **Changed** lists ids whose content differs from the previous release.
- **Retired** lists ids whose status became `retired`, or that no longer appear.

Each line reads `L-NNN: title`, and a section with nothing in it is left out. An entry ends with a line giving the number of public cases and lessons as of that release. The changelog itemises lessons only; cases appear in it only through that count.

## Reading and using it

People read [lessons.md](lessons.md). To see the cases behind a lesson, take the `C-NNN` ids on its `cases:` line and look them up in `findings.json`, where each row carries its id in `case`.

Tools should fetch the JSON files from a tag, not from the default branch, so the data cannot change underneath them. In a clone, `git checkout lessons-vYYYY.MM.N` pins the whole working tree to one release. To fetch a single file, request it at the tag:

```text
https://raw.githubusercontent.com/celestial-artifact-labs/usability-lessons/lessons-vYYYY.MM.N/lessons.json
```

The files refer to each other by id:

- A case's `lessons` field lists `L-NNN` ids. They match the `## L-NNN` headings in `lessons.md`.
- A lesson's `cases` field lists `C-NNN` ids. They match the `case` field of rows in `findings.json`.
- Rows in `lessons.json` carry no id. `lessons.md` and `lessons.json` list the same lessons in the same order, sorted by title and not by id, so the nth `## L-NNN` heading in `lessons.md` belongs to the nth row of `lessons.json`.

## The lesson files

`lessons.md` begins with a short header giving `version` and `generated`. It then has one section per lesson, headed `## L-NNN`. Each section gives the title, the `theme` and the `status`, then three parts headed rule, why and how to check, and ends with a `cases:` line and an `earned_from_count:` line. The `cases:` line reads `none exposed yet` when a lesson has no public case.

`lessons.json` is an array with one object per lesson:

| Field | Meaning |
| --- | --- |
| `title` | A short statement of the lesson. |
| `theme` | One of the six themes below. |
| `status` | `candidate`, `active` or `retired`. |
| `rule` | The rule itself. |
| `why` | Why the rule holds, including why the mistake keeps recurring. |
| `how_to_check` | A step someone else can repeat to check that the rule was followed. |
| `cases` | The `C-NNN` ids of the public cases the lesson was learned from. It can be empty. |
| `earned_from_count` | How many records the lesson was learned from, including any that have no public case. It can be larger than the length of `cases`, and it can be 0. |

Lesson statuses:

- `candidate`: proposed, and not yet confirmed: no second record has confirmed it, and no single record has shown that it plainly holds beyond the technology it was found in.
- `active`: confirmed, either by a second record from any project or by one record that plainly holds beyond its stack. An active lesson can therefore rest on a single record.
- `retired`: no longer holds. A retired lesson stays in the files with that status.

Themes, which the `tags` field of a case also uses:

| Theme | What it covers |
| --- | --- |
| `feedback` | The system did, or did not, tell the user that an action happened. |
| `layout` | Sizing, spacing or viewport assumptions that break on a real screen. |
| `controls-and-state` | A control's visible state does not match what a user can actually do with it. |
| `colour-and-contrast` | A colour choice fails to communicate meaning, or fails a contrast check against its real background. |
| `vocabulary-and-copy` | Wording that is ambiguous, inconsistent or unclear at the decisive moment. |
| `method` | A lesson about how usability work itself is done, not about one interface trait. |

## The findings file

`findings.json` is an array. Full case rows come first, ordered by case id, and every one has the same keys in the same order:

| Field | Meaning |
| --- | --- |
| `case` | The public id, `C-NNN`. It is assigned once, never reused and never renumbered, and says nothing about product or year. Gaps in the sequence are expected: a case that cannot be told without naming its product is withheld rather than weakened. |
| `title` | A neutral one-line title. |
| `surface` | The kind of interface involved, such as a checkout, an admin screen or a game. It is a class of interface, never a named product. |
| `severity` | `1` to `4` on the scale below, or `null` for a preventive case. |
| `preventive` | `true` for a case that records a decision taken ahead of a failure to keep it from happening. A preventive case carries no severity. |
| `status` | `open` (not yet decided or scheduled), `planned` (a fix is decided but not shipped), `fixed` or `wont-fix` (a deliberate decision not to fix). All four are published, so the record shows its own unresolved failures. |
| `observed` | The month the problem was observed, `YYYY-MM`. Exact dates are not published. |
| `method` | How the problem was found: `user-report`, `self-test`, `browser-audit`, `screenshot`, `code-read`, `automated-check` or `design-review`. `design-review` marks a preventive case. |
| `backfilled` | `true` when the case was reconstructed afterwards from an earlier dated source rather than recorded as it happened. Such a case claims nothing beyond what its source supports. |
| `heuristics` | The usability principles the case bears on, as identifiers: `nielsen-1` to `nielsen-10` for the ten usability heuristics, `wcag-X.Y.Z` for a WCAG success criterion, and names starting `law-` or `principle-`. |
| `tags` | Theme labels, from the six themes above. |
| `lessons` | The `L-NNN` ids of the lessons this case helped establish. It can be empty. |
| `evidence_kinds` | Which kinds of citation the private record rests on, any of `path`, `commit`, `measurement` and `quote`. The export derives it; nobody types it. |
| `observation_digest` | `sha256:` followed by 64 hexadecimal characters. See the section "Why there are no evidence links" below. |
| `goal` | Who was trying to do what, described by role, and on what kind of surface. |
| `what_went_wrong` | The behaviour, with the numbers, and the mechanism at the level of the platform. |
| `why_it_matters` | How often, how badly and how persistently the problem affects, or would affect, people. It ends with the severity name as its own word (Blocking, Serious, Moderate or Cosmetic); a preventive case ends without one. |
| `resolution` | How it ended or what is being considered, including what was rejected and why where that was recorded. |

A case that shipped in a tagged release and was later withdrawn is not removed. It is appended after the full rows, at the end of the array, as `{ "case": "C-NNN", "withdrawn": true }` with no other keys. Its id remains resolvable and is never reused.

## The manifest

`manifest.json` has these fields:

- `profile`: `public`, which marks the file as the public export.
- `generated`: the date of the snapshot, which sets the year and month of the release version.
- `version`: a date stamp of the snapshot, as described under "Versions and the changelog" above.
- `counts`: the number of public cases (`findings`) and lessons, plus the cases broken down by surface (`by_surface`) and by severity level (`by_severity`). `counts.findings` counts full case rows only, so it can be smaller than the length of `findings.json` when withdrawn cases are present. Preventive cases carry no severity and are left out of the severity counts.

## What a public case is

A public case is written fresh, in project-free words, from a private record that holds the real evidence. It is not a redaction of that record. The private record keeps the names, paths, commits and links; none of that is published. What leaves is the case text, its neutral title, its rating and a few fixed fields.

Each case describes the person by role, the interaction by what it did, and the mechanism at the level of the platform. It uses nothing from the product's own vocabulary. The mechanical check refuses text that contains a path, a commit, a URL, a repository or folder name, a capitalised product word, a term the maintainer has declared to be product vocabulary, or a run of four or more words copied from the private evidence. A case that cannot be told without identifying its product is withheld, not watered down.

## Why there are no evidence links

Nothing in a case points to anything you can open, so there is no link to a commit, a file or a page. Anonymity here withholds detail; it does not blur it. Four things say that a checked record stands behind each case:

1. A case exists only because a private record exists. That record cannot exist without at least one concrete citation (a file and line, a commit, a measurement or a dated quotation), it carries a severity confirmed by the one person who rates every case (a preventive record carries none), and its observation layer is never rewritten; corrections are appended.
2. `evidence_kinds` says which kinds of citation the private record has. The export derives it, so it cannot be invented.
3. Measurements travel with the case. If the private evidence holds a measurement, at least one measurement from it must appear in the case. A mechanical check also refuses a case that contains a pixel, millisecond, percentage or contrast-ratio figure not found in the private record's text. A case may leave other measurements out.
4. `observation_digest` is a SHA-256 hash over the parts of the private record that are frozen once written: its evidence, its account of what the user experienced, and when, by whom and how it was observed. It reveals nothing without the text. If the maintainer later discloses one private record, the hash lets a doubter confirm that this is the record the case was built from and that it was not rewritten after publication.

The digest is a commitment, not a proof to a stranger: it cannot show that a record is genuine until that record is disclosed. The tagged release is the attestation; there is no separate sign-off field.

## Severity scale

Severity measures user impact, not implementation effort.

| Level | Name | Definition |
| --- | --- | --- |
| 1 | Blocking | The user cannot complete the task, or completes it incorrectly without knowing. |
| 2 | Serious | The task is completable but the user is likely to fail, retry, or misread. |
| 3 | Moderate | Friction; the user recovers unaided. |
| 4 | Cosmetic | Noticed but harmless. |

An accessibility case takes the severity of the experience it causes, not the severity of the guideline it touches. Preventive cases carry no severity and are left out of roll-ups. The maintainer of the private ledger rates every case, which is what keeps severities comparable from one case to the next.

## Ownership

Maintained under the [celestial-artifact-labs](https://github.com/celestial-artifact-labs) GitHub organization. To report an error in a lesson or a case, or to ask a question about the data, [open an issue](https://github.com/celestial-artifact-labs/usability-lessons/issues).

## License

Released under [CC0 1.0 Universal](LICENSE), a public-domain dedication. You may copy, modify and redistribute everything in this repository, including for commercial purposes, without asking permission and without crediting anyone. The full text is in the [LICENSE](LICENSE) file.
