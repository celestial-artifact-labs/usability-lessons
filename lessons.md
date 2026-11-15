# public usability lessons

version: 2026.11.06
generated: 2026-11-06

---

## L-002

Announce every state change through a live region that already exists

theme: feedback
status: active

### rule

A state change that only a sighted user can perceive is an unannounced state change. `aria-live` on a region that already exists, not a region that mounts with its content.

### why

Assistive technology observes mutations to a live region that is already in the accessibility tree. A region that is inserted together with its text is frequently not announced at all, because there was no prior state to diff against. The mistake recurs because the conditional-render version tests green by eye: the text appears on screen, so the feature looks delivered, and the missing announcement is invisible to everyone who can see.

### how to check

Render the live region empty at mount and fill it on the action. Then run the flow with a screen reader and confirm the announcement fires on the second and third activation, not only the first.

cases: C-023
earned_from_count: 1

---

## L-015

Ask for one decision at a time, and say what the answer changes

theme: vocabulary-and-copy
status: candidate

### rule

A request carrying two decisions is not a request. Ask for one decision at a time, name the options, say what happens after each, and expand any term the reader has not used themselves.

### why

Bundling is cheap for the asker and expensive for the reader. The asker holds both decisions in mind already, so joining them costs nothing and saves a paragraph; the reader has to separate them, work out which parts of the reply belong to which, and guess what form each answer should take. When the two decisions need different kinds of answer, one a judgement and one a yes or no, there is no single reply that fits, so the reader either answers half of it or asks what the question was.

The mistake recurs at the end of a turn, where a summary is being compressed and open items get swept into one closing sentence. That is the worst place for it: it is the sentence most likely to be read quickly, and the one the reader is expected to act on.

### how to check

Read the request and count the decisions the reader has to make. More than one means split it. Then, for each remaining decision, check that a reader could write their answer without asking a clarifying question first, and that every term in it either appeared in their own words earlier or is expanded on the spot.

Where the request is a UI rather than a sentence, the same count applies to a dialog, a form step, or a confirmation prompt.

cases: C-024
earned_from_count: 1

---

## L-013

Ask whether a dead or wrong rule was ever right before restoring it

theme: method
status: active

### rule

When you find a dead or wrong rule, ask whether it was ever right before restoring it.

### why

Finding dead code frames the problem as a connection that was lost, so the obvious repair is to reconnect it. If the rule was already wrong when it was written, reconnecting it ships the original defect with a fresh commit message and a clean conscience. The mistake recurs because the discovery is satisfying enough to substitute for the judgement.

### how to check

Before restoring a rule, evaluate it as if it were new: apply it in isolation and measure the result. Restore only if it passes on its own merits.

cases: C-018
earned_from_count: 1

---

## L-016

Check contrast against the surface an element actually renders on, not the one it was designed or declared against

theme: colour-and-contrast
status: active

### rule

Verify contrast against the background an element actually resolves to in the shipped product, in every state it can appear in, rather than against the canvas, page default, or declared style it was designed, authored, or approved against.

### why

Contrast gets checked at the moment and place where it is easiest to check: a design canvas before an asset ships, a stylesheet rule as written, or a page’s default background. But the background a piece of text or an asset actually sits on is decided later and elsewhere: by whether the selector meant to style it actually matches the rendered markup, by which state a component is in (a highlighted or tinted row versus its default), or by which page’s theme the asset ships into. Nothing in a build, a type checker, or a manual review re-verifies contrast at that later, resolved point, so a gap between the surface that was checked and the surface that renders can survive all the way to production. This recurs across projects that share no source code, and even outside code entirely in a design asset, because the failure is a process gap in when and against what contrast gets checked, not something these three findings show tied to one shared stack.

### how to check

In the live build, sample the element’s actual computed background colour in every state it can appear in (default, active or selected, any severity-tinted state, and on every page or theme it ships into), and compute the contrast ratio against that sampled surface, not against the design file, the declared CSS rule, or one page’s default background.

cases: C-001, C-004, C-018
earned_from_count: 3

---

## L-009

Confirm a control's value survives the whole write path before calling it delivered

theme: controls-and-state
status: active

### rule

A UI affordance is not delivered until the value it sets survives the entire write path. Check the server before calling the checkbox done.

### why

A control is verified where it is built, in the browser, where its state is visibly correct. Everything between that state and storage, request shaping, whitelists, defaults, document builders, can drop the field without erroring, and each of those layers was written before the control existed. The mistake recurs because the visible half of the feature is the half under test.

### how to check

Set the control to its non-default value, save, then read the persisted record from the server or the database. If the stored value does not match what was set, the affordance is not delivered.

cases: C-020, C-025
earned_from_count: 2

---

## L-004

Distinguish failure causes instead of collapsing them into the most common one

theme: feedback
status: active

### rule

Mapping every failure to its most common cause lies to the user and hides the real bug. Distinguish a wrong password from an unreachable database.

### why

A catch-all around an operation that can fail for several reasons produces one message for all of them, and the message chosen is always the expected reason. The user then acts on a diagnosis that is wrong, and the team reads a log that agrees with the wrong diagnosis. The mistake recurs because the catch-all is defensive coding that looks responsible, and because the common case really is common, so the wrong label is right most of the time.

### how to check

List the distinct causes the operation can fail for, then read the catch block. If two causes leave through the same status code and the same string, the taxonomy is missing.

cases: C-022
earned_from_count: 1

---

## L-017

Match each interface word to exactly one underlying cause or state, and record any deliberate exception

theme: vocabulary-and-copy
status: active

### rule

Match each word used in the interface (an error message, a control label, or a confirmation) to exactly one underlying cause or state: never let a single message stand in for several different failures, and never let a control and its own confirmation, or a themed word substituted for a plain one, name the same real-world transition or action with two different words without recording that the two words are a deliberate choice, especially at a decisive moment such as authentication, a state change, or a payment.

### why

The mismatch happens two distinct ways, both breaking the one-to-one link between a word and the reality it names. First, a catch-all lets one message stand in for several different underlying causes: C-022’s token-verification catch-all mapped connection, timeout, and driver errors all to the same 401 response blaming the credentials, so a user hitting a database outage read that their password was wrong and retried a correct one uselessly. The single message is read as diagnostic, and the user acts on whatever cause it names even when that cause is wrong. Second, one real-world transition or action ends up named by two different words because each word is locally true for the narrow thing it labels: in C-017, a control’s verb and the confirmation that follows it are both accurate, one naming the operator’s action and the other the system’s resulting state, and the finding records this as a deliberate, reconciled choice rather than an oversight; in C-006, a themed label on the pay button was locally consistent with the page’s fiction. In both cases the reader has only the words to judge by, not the reasoning behind them, so a pair of words that are each individually defensible can still read as two different events, or leave a first-time buyer without a plain-language reference for what the label refers to. Both mechanisms recur across projects that share no code because the failure sits entirely in the copy layer, what a message or label names, rather than in any framework mechanism.

### how to check

List every word the interface uses for a failure, a control, and its confirmation on the surface under review, and for each ask which single underlying cause or state it names. Flag any message that covers more than one cause, and any transition named by two different words across a control and its confirmation. Where two words are kept on purpose, require the decision to be written down in the record, so the next reader finds a vocabulary rather than an accident.

cases: C-006, C-017, C-022
earned_from_count: 3

---

## L-005

Measure percentage and viewport widths on content containers at 320px

theme: layout
status: active

### rule

Percentage and viewport widths on content containers are desktop assumptions. Two separate findings, a month apart, in different files, both landed on *about 96px of readable content at 320px* — a 30% width on the hero and a 30vw width on the payment card. When you see a percentage width on a text container, measure it at 320px before anything else.

### why

A relative width is written while looking at a wide window, where thirty percent is a comfortable column. The same declaration on a phone yields a ribbon, and nothing in the stylesheet shows it, because the failing number never appears in the source. The mistake recurs because the value is chosen visually at one viewport and then never re-evaluated at the smallest one.

### how to check

Grep the stylesheets for a percentage or viewport width on any element that holds text, then load that surface at a 320px viewport and read the rendered width. A text container that lands near the 96px both findings measured has the same defect; record the measured width either way.

cases: C-009, C-018
earned_from_count: 2

---

## L-011

Never let scaffolding mutate real user state, and leave a test asserting its absence

theme: controls-and-state
status: active

### rule

Scaffolding that mutates real user state is the most dangerous kind, because it looks like a feature. Leave a test asserting the absence.

### why

Placeholder code that writes to a cart, an account, or an order is indistinguishable from a working feature when the page is opened: something happens, and something happening is what a feature looks like. Nothing flags it later, because no test asserts that an action does not occur. The mistake recurs because removal leaves no trace, so a future refactor can reintroduce it with the same innocence.

### how to check

For each removed mutation, add a test asserting the action is never dispatched on mount, and confirm it fails when the mutation is put back.

cases: C-008
earned_from_count: 1

---

## L-003

Never show the user an error naming a value they never typed

theme: feedback
status: active

### rule

An error message that names a value the user never typed is a defect report wearing a user message's clothes.

### why

Validation messages are written from the validator's point of view, where the offending value is the most informative thing available. From the user's point of view a value they never entered is noise at best and an accusation at worst, and it gives them no action to take. The mistake recurs because interpolating the rejected value into the message is the shortest path to a message that helps the developer.

### how to check

Read every error string the surface can produce and ask, for each interpolated value, whether the user typed it. If they did not, the message names the wrong subject.

cases: C-021
earned_from_count: 1

---

## L-001

Put feedback for an action where the action happened

theme: feedback
status: active

### rule

Feedback for an action belongs where the action happened. A count in the header is a reference, not a confirmation.

### why

A persistent counter is ambient state: it is correct at every moment and therefore announces no moment in particular. A person who presses a button is asking one question, "did that work", and they ask it while looking at the button. Answering somewhere else costs them a search, and the search only starts if they already suspect the answer exists. The mistake recurs because the counter is genuinely useful and genuinely updates, so the code looks finished from the developer's side of the screen.

### how to check

Press the control and, without moving your eyes from it, name what changed. If the only change is outside the region you were looking at, the feedback is in the wrong place.

cases: C-023
earned_from_count: 1

---

## L-014

Record what was rejected and why

theme: method
status: candidate

### rule

Record what was rejected and why. The reasoning behind a rejected fix is often more reusable than the fix that shipped.

### why

A shipped fix is recoverable from the code; a rejected one exists only in whoever considered it. The rejected option is usually the obvious one, so the next person meets it first and has to re-derive the objection, or does not, and ships it. The mistake recurs because writing down a decision that produced no diff feels like writing down nothing.

### how to check

Read the record’s decision section. If it names only what was done, the alternatives that were considered and dropped are missing.

cases: none exposed yet
earned_from_count: 0

---

## L-008

Render a damage-preventing control in create mode, not only in edit mode

theme: controls-and-state
status: active

### rule

A control that appears only in edit mode is missing from the moment it prevents damage.

### why

Edit forms are usually built first from an existing record, so every field the record carries has somewhere to live. The create form is then derived by removing what cannot be known yet, and a safety control gets removed with them, even though creation is the step where the damage actually happens. The mistake recurs because both forms look complete on their own.

### how to check

List the controls on the edit form and the create form side by side. For every control present on only one of them, state which failure it prevents and confirm that failure cannot occur on the other.

cases: C-020
earned_from_count: 1

---

## L-006

Stop claiming the viewport in a route stylesheet when a shell already owns it

theme: layout
status: active

### rule

When a shell owns the viewport, route stylesheets must stop claiming it. `min-height: 100vh` inside a filled shell is additive.

### why

Full-height declarations compose by addition, not by agreement: a shell that already fills the window plus a child that also demands a full window produces a page taller than the window, with nothing in the extra space. The mistake recurs whenever a layout shell is introduced after the routes were written, because every route stylesheet keeps the rule that was correct before the shell existed.

### how to check

After introducing or changing a layout shell, grep every route stylesheet for a full-viewport height or width, then compare `document.documentElement.scrollHeight` with `window.innerHeight` on a page with no content below the fold. They should be equal.

cases: C-019
earned_from_count: 1

---

## L-007

Verify that a stylesheet's outermost selector is actually rendered

theme: layout
status: active

### rule

A stylesheet nested under a class that is never rendered fails silently and completely. Nothing in the build, the type checker, or the test suite catches it.

### why

A nested block compiles to a descendant selector that is valid CSS and matches nothing. There is no error to raise: the compiler cannot know which classes the component tree emits, the type checker never sees the stylesheet, and a unit test that stubs style imports asserts nothing about them either. The mistake recurs after a rename or a refactor of the markup, which leaves the stylesheet syntactically perfect and functionally dead.

### how to check

For each stylesheet, take its outermost selector and grep the component tree for that class name. No match means the file styles nothing. Confirm in the browser by toggling one declaration and watching for a visible change.

cases: C-018
earned_from_count: 1

---

## L-012

Weight a finding from a real, visible browser above a finding from a test suite that never renders one

theme: method
status: active

### rule

Weight a finding produced by driving a real, visible browser above a finding produced by running a test suite that never renders and drives a real page.

### why

A test runner proves only what it actually exercises. Cross-origin (CORS) enforcement and frame or visibility timing are behaviors the browser itself supplies, so a suite that never drives a real, visible browser page can stay green while one of those behaviors is broken, or simply never runs at all. C-016 and C-010 demonstrate this in two projects that share no code: a test suite that calls a server’s routes directly from the test process never passes through the browser’s own CORS preflight enforcement, so a route that only failed from a real origin stayed green until a browser drove it; and motion coded inside a `requestAnimationFrame` loop stops running the instant the tab is backgrounded or embedded, a state that project’s own done criteria for its smoke test did not require until after the fix, so the loop was already producing zero frames in exactly that state with no suite result to contradict it. Both projects sit on the same front-end framework, but neither defect is a framework behavior, so the shared framework is incidental rather than the mechanism. The three findings this rule was first earned from, all in a third project, were each surfaced by the same browser pass rather than by the suites already covering that code: C-020 by a scripted browser audit driving the real create, edit and delete lifecycle, and C-021 and C-022 from the live admin form during that same pass. They establish that a browser pass finds real defects a team had not otherwise found; they do not establish that a suite could not have found them, which is the sharper claim the first two findings carry. Different mechanisms, same shape: the runner never entered the state where the defect lives, so it could not have found it. A green suite then reads as evidence of correctness rather than as evidence about the narrow slice of behavior the suite was ever in a position to see.

### how to check

Run the surface end to end in a real, visible browser window (`document.visibilityState === "visible"`, not a hidden or embedded preview pane), driving any HTTP control from the page’s own origin rather than calling it directly from the test process, and record browser, viewport, and window state with the finding. Anywhere the suite stubs a stylesheet, calls a route directly instead of through the browser, or never puts the page into a backgrounded or embedded state, treat that area as untested regardless of the suite’s result.

cases: C-010, C-016, C-020, C-021, C-022
earned_from_count: 5

---

## L-010

When an application rejects its own output, fix the acceptor, not the producer

theme: controls-and-state
status: active

### rule

When an application rejects its own output, fix the acceptor, not the producer.

### why

The rejection surfaces at the acceptor, so the value looks wrong and the producer looks like the culprit. Changing the producer to satisfy a validator usually means encoding an assumption the producer had deliberately avoided, such as an absolute host, and that assumption then fails in the next environment. The mistake recurs because the producer is easier to change and the error message points away from the validator.

### how to check

Trace the rejected value back to the code that produced it. If that code is inside the same application, the validator is the thing describing reality incorrectly.

cases: C-021
earned_from_count: 1

---
