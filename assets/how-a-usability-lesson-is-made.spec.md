## Concept spec: how-a-usability-lesson-is-made

- **Idea (one sentence):** A usability failure is recorded once, distilled into one rule, and published so the next person is warned, and four skills are the calls a crew makes along the way.
- **Audience:** readers of this repository's README who design, build or review interfaces and have not seen the private ledger.
- **Medium:** doc figure, 1600 x 880 (about 16:9).
- **Title:** How a usability *lesson* is made
- **Subtitle:** Every grounding is charted once, so the next vessel sails clear.
- **Metaphor:** a coast with a rock. A ship that runs aground is a person failed by a surface; the logbook is the finding; the chart is the lesson; the lighthouse is the published plugin. The mapping holds because a hazard charted once warns every later ship.
- **Runner-up metaphor:** a kitchen: a burnt dish, the note on the recipe card, the house recipe, the card pinned by the stove.
- **Archetype:** pipeline
- **Cast:**

  | Object | Stands for | Drawn as |
  | ------ | ---------- | -------- |
  | Grounded ship and rock | a person failed by a surface | line drawing, tilted hull |
  | Open logbook | a finding with evidence | open book with a pen |
  | Sea chart | a lesson | chart with a marked X |
  | Lighthouse | the plugin and its guard | lighthouse with a beam |
  | Upright ship | the next piece of work | line drawing, level hull |
  | Signal flags | the four skills | small teal pennants beside each command |

- **Relationships:**

  | From | To | Verb phrase |
  | ---- | -- | ----------- |
  | Grounding | Logbook | is recorded as |
  | Logbook | Chart | is distilled into |
  | Chart | Lighthouse | is published to |
  | Lighthouse | Next vessel | warns |

- **Composition:** five stations left to right on one waterline; under each, a caption and, where a skill or actor does the step, a pill; below a rule, the four skills in the order a crew uses them; the takeaway at the bottom left.
- **Style preset:** flat line art on deep blue, taken from the ledger's lighthouse poster.
- **Palette:** #2F39A9 sea and sky; #E8F2FB line and text; #15D8B3 accent, used for the title word, the light, the flags and the kickers; #49A4BB rules and connectors; #263091 pill fill.
- **Text inventory:**
  1. How a usability lesson is made
  2. Every grounding is charted once, so the next vessel sails clear.
  3. FIVE STATIONS · FOUR SKILLS · ONE KEEPER
  4. I · THE GROUNDING / II · THE LOGBOOK / III · THE CHART / IV · THE LIGHTHOUSE / THE NEXT VESSEL
  5. is recorded as / is distilled into / is published to / warns
  6. Someone is failed by a surface. / Written down once, with evidence. / One reusable rule, marked once. / Carried into the next work. / Warned before it nears the rock.
  7. FINDINGS/ / LESSONS/ / A PLUGIN
  8. /log-usability-finding / The keeper rates and distils. / Its guard warns on its own. / /usability-lessons
  9. THE CREW'S CALLS · THE FOUR SKILLS YOU TYPE / ★ ALSO IN THE PUBLIC PLUGIN
  10. /usability-init: Hoists your colours. Once per repo: declares it to the ledger.
  11. /usability-lessons ★: Reads the chart before you sail. Before work: the lessons for one surface.
  12. /log-usability-finding: Writes the grounding in the logbook. After a failure: evidence and a lesson.
  13. /usability-setup: Pins the chart's address aboard. Optional: points your docs at the ledger.
  14. The point: no one runs aground twice in the same place.
  15. RECORD · DISTIL · RECOMMEND · GUARD
- **Takeaway line:** The point: no one runs aground twice in the same place.
- **Renderer decision:** svg. The text inventory is label-dense, and no image tool was named.

## To iterate

- Different wording for a skill: edit its `<g id="skill-N">` group in the SVG, or item 10 to 13 above.
- Fewer labels: remove `<g id="zone-station-skills">` (the pills under the stations); the skills row still names all four.
- Fonts: the file asks for Cormorant Garamond and Lora and falls back to Georgia where they are not installed, which is how the PNG was rendered.
