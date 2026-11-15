# Changelog

All notable changes to the published usability lessons are recorded here,
newest first.

Versions are Calver `YYYY.MM.N`: the year and month of the `generated` date
in `manifest.json`, then a sequence number that starts at 1 each month and
rises only when a second release ships in the same month. Because that date
belongs to the snapshot itself, the month can trail the day a release is
published; it is never the day any lesson was written or last edited.

Each entry is built by comparing this release's `lessons.json` with the
previous release's, by lesson id (`L-NNN`, stable and never renumbered):
**Added** for an id not seen before, **Changed** for an id whose fields
moved, **Retired** for an id whose status became `retired` or that
disappeared entirely. A section is left out when it is empty. Everything
below this introduction is written by the release tooling, which adds each
new entry at the top and leaves this introduction alone.

## 2026.11.1 - 2026-11-06

### Added

- L-001: Put feedback for an action where the action happened
- L-002: Announce every state change through a live region that already exists
- L-003: Never show the user an error naming a value they never typed
- L-004: Distinguish failure causes instead of collapsing them into the most common one
- L-005: Measure percentage and viewport widths on content containers at 320px
- L-006: Stop claiming the viewport in a route stylesheet when a shell already owns it
- L-007: Verify that a stylesheet's outermost selector is actually rendered
- L-008: Render a damage-preventing control in create mode, not only in edit mode
- L-009: Confirm a control's value survives the whole write path before calling it delivered
- L-010: When an application rejects its own output, fix the acceptor, not the producer
- L-011: Never let scaffolding mutate real user state, and leave a test asserting its absence
- L-012: Weight a finding from a real, visible browser above a finding from a test suite that never renders one
- L-013: Ask whether a dead or wrong rule was ever right before restoring it
- L-014: Record what was rejected and why
- L-015: Ask for one decision at a time, and say what the answer changes
- L-016: Check contrast against the surface an element actually renders on, not the one it was designed or declared against
- L-017: Match each interface word to exactly one underlying cause or state, and record any deliberate exception

Counts as of this release: 23 public case(s), 17 public lesson(s).
