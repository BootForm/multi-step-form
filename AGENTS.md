# Working in this repo

This is a teaching template for a multi-step form/questionnaire, in the same "no build step, one
page" family as `first-website` and `one-page-site`. Someone clicks "Use this template", edits
one file, and gets a live multi-step form with a working submission. Keep it that way.

## The constraints that define this repo

- **No build step, no package manager, no terminal required to use it.** Everything must work
  when the reader edits files through GitHub's web editor and GitHub Pages serves the repo root
  as-is. Do not add `package.json`, a bundler, or a framework.
- **One page, one file.** `index.html` is the whole thing: markup, styling, and the step logic, in
  a single `<script>` at the bottom. If a change needs a second file, it probably belongs in a
  different template.
- **Tailwind via the browser CDN** (`@tailwindcss/browser@4`), the same as `first-website`.
- **Comments in `index.html` are the lesson**, not clutter. Keep `CHANGE ME` markers where the
  reader is meant to edit.

## A `hidden` field is still validated; only a `disabled` one is genuinely excluded

The whole step mechanism depends on this, and it's easy to get backwards. **Confirmed against a
real DOM implementation while building this, not assumed**: a required `<input>`/`<select>`
inside an ancestor `<div hidden>` still counts toward `form.checkValidity()` /
`form.reportValidity()`, exactly as if it were visible. Only `disabled` (or a `<fieldset
disabled>` ancestor) genuinely removes an element from constraint validation. This means "hide
every step but the current one, then call `form.reportValidity()`" does **not** validate just the
current step. It validates the whole form every time, and blocks advancing past step 1 the moment
any later, still-empty required field exists anywhere in the form.

**The fix, already in `index.html`'s `nextBtn` handler**: before validating, temporarily set
`disabled = true` on every field outside the current step, call `form.reportValidity()`, then set
`disabled = false` again immediately after, regardless of the result. This is the one piece of
logic in this template that isn't obvious from reading the HTML; don't simplify it back to a bare
`form.reportValidity()` call, even though that reads cleaner, since it silently breaks step
gating with no error, only wrong behavior on click.

**How this was actually caught**: this repo has no test suite (a single static HTML file doesn't
warrant one), but the step logic was verified with a throwaway Node script using `jsdom` before
publishing (not committed here, since it's not a build artifact this repo needs), simulating
clicks and checking which step ended up visible. That's what caught the bug above: a version that
looked fine in the editor, and would have looked fine in a screenshot of the resting page, failed
every single "click Next" assertion. If you change the step logic, re-verify with something that
actually drives the JavaScript (a real click in a real browser, or a similar script), not just a
visual read of the HTML.

## The confirmation step's summary is a separate mechanism from `SUMMARY_FIELDS`

The last step is a read-only review of everything entered so far, not a new set of fields.
`renderSummary()` runs once, right when the reader lands on that step (see the `if (onLastStep)`
check in `render()`), and rebuilds it from a fresh `new FormData(form)` read every time, so it
always reflects whatever's currently in the earlier steps, including anything changed after going
Back and forward again.

**Adding a field to an earlier step doesn't automatically show it in the summary.** The
`SUMMARY_FIELDS` object at the top of the script is a deliberate allowlist, keyed by each field's
`name` attribute, mapping it to the label to show. A field left out (like the honeypot, on
purpose) is still submitted, it just doesn't appear in the review. Add a new entry there when you
add a new field elsewhere, in the order you want it to appear.

## The form

The `action` ships as the literal placeholder `https://f.bootform.com/__YOUR_FORM_ID__`.

**In this template repo (`github.com/BootForm/multi-step-form`), it stays the placeholder.** A form ID is the
entire claim credential: whoever claims one first owns it permanently, and every copy of the
template would then deliver its visitors' messages to that person.

**In a copy of the template (someone's own site), the opposite applies.** Putting the owner's own
form ID in the `action` is step 3 of the README, and it is how their form starts working. If
the `action` already holds a real ID, it is theirs: keep it, and never change it back to the
placeholder. Only ever use an ID the owner generated (never one from a doc, a tutorial or another
site). To tell which case you are in, run `git remote get-url origin`: only the template itself
points at `BootForm/multi-step-form`.

The honeypot input, named `honeypot`, must stay, hidden and unlabelled to screen readers.

Submission is AJAX (`fetch`, `Accept: application/json`), not a plain form POST that navigates
away: a multi-step form's whole point is staying on one page, so leaving it after all that effort
just to show "thanks" would undercut the format. This does mean the success/error message is
hand-rolled in the `<script>` rather than BootForm's own default redirect/thank-you handling.

## Writing style

- Second person, present tense, short sentences.
- **No em dashes or en dashes.** Use a comma, a colon, a full stop, or brackets.
- No AI attribution in commits or pull requests, here or anywhere else in this organisation.

## Before changing anything

Read the step-navigation script fully before touching it; the disable/re-enable trick above is
load-bearing and not visually obvious from the markup alone. If you change it, re-verify with an
actual click, not a read.
