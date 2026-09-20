# A multi-step form or questionnaire, no installs needed

A single page, no build step, no framework: a 5-step questionnaire (what you need, budget and
timeline, who you are, any details, then a review of everything you entered) that stays on one
page the whole way through and ends in a real submission, not a dead end.

**[See what you are building →](https://bootform.github.io/multi-step-form/)**

---

## Before you start

You need a free GitHub account. That is it. Everything below happens in your browser.

---

## Step 1: Make your own copy

Click the green **Use this template** button at the top of this page, then **Create a new
repository**. Name it `multi-step-form`, or whatever you like.

---

## Step 2: Put it on the internet

In *your* new repository:

1. Click **Settings** (the tab along the top).
2. Click **Pages** in the left sidebar.
3. Under **Source**, choose **Deploy from a branch**.
4. Set the branch to **main** and the folder to **/ (root)**. Click **Save**.

Wait about a minute, then reload that Settings page. A green box appears at the top with your
address, something like `https://yourname.github.io/multi-step-form/`.

**Open it.** Click through the steps. It still asks about a generic project. We will fix that
next.

---

## Step 3: Make it yours

Click **`index.html`**, then the pencil icon (✏️) to edit it. Look for the lines marked `CHANGE
ME`:

- **The title and description**, near the top.
- **Two colours**, just below it. `--color-ink` is the text, `--color-paper` is the background,
  `--color-accent` is the progress bar, buttons, and focus rings.
- **The heading and intro** at the top of the page.

Then change the actual questions: the four options in the form's own first step, the budget
ranges in its second step, and the field labels throughout. Add or remove an option by copying one
of the existing `<label>` blocks (first step) or `<option>` lines (second step's select).

The last step is a read-only review of everything entered, built automatically from the
`SUMMARY_FIELDS` list near the top of the `<script>` tag. **Add a field somewhere else in the form
and it won't show up in the review until you also add it there**: an object mapping that field's
`name` attribute to the label you want shown, in the order you want it to appear.

**Want more or fewer steps before the review?** Copy an entire `<div data-step hidden>...</div>`
block to add one, or delete one to remove it. The step-navigation script counts `[data-step]`
elements automatically; you don't need to touch the JavaScript to change how many steps there
are, only the markup (and `SUMMARY_FIELDS`, if the step has fields worth showing on the review).

Commit your changes, wait a minute, and reload.

> **Picking a colour is the hard part.** If you have no idea, `--color-accent: #2563eb` (a plain
> blue) is a safe default.

---

## Step 4: Make the submission actually work

Here is the bit most tutorials skip.

Your page is a *static* site. There is no program running behind it, so there is nowhere for a
form to send anything. We will use [BootForm](https://bootform.com), because you can point a form
at it and it works immediately, with no account.

**4a. Generate your form ID.**

It is just a random UUID, and it has to be yours alone. Open BootForm's own [UUID
generator](https://bootform.com/uuidgenerator) and copy what it shows you (or run
`crypto.randomUUID()` in any browser console). You'll get something like
`11111111-1111-4111-8111-111111111111`, though a real one won't repeat digits like that.

> **Use your own.** Do not use the one printed above, and do not use a friend's. Whoever claims a
> form ID first owns it, and everything sent to it goes to them. Yours should be a fresh random one
> that nobody else has seen.

**4b. Paste it into `index.html`**, replacing `__YOUR_FORM_ID__` in the `action` attribute.

**4c. Try it, then claim it** at the claim link BootForm gives you after your first real
submission, **before** the site goes live publicly. Held submissions are kept for 48 hours before
they're deleted.

Unlike a plain HTML form, this one submits over `fetch` and shows a "thanks" message right there
on the page instead of redirecting anywhere, since a multi-step form's whole point is staying on
one page. If something goes wrong, the actual error shows up the same way, not as raw JSON.

---

## That's it

You have a real multi-step form, staying on one page the whole way through, with a submission
that actually goes somewhere.

### Where to go next

| | |
|---|---|
| **More input types** | Date pickers, phone numbers, file upload, checkboxes: [`form-styles`](https://bootform.github.io/form-styles/). |
| **A proper domain** | `yourname.com` instead of `github.io`. [one-page-site's Step 5](https://github.com/BootForm/one-page-site#step-5-put-it-on-your-own-domain) walks through it. |
| **A bigger site around this form** | [one-page-site](https://github.com/BootForm/one-page-site) or [vitepress-marketing](https://github.com/BootForm/vitepress-marketing) for a full site with this questionnaire as its contact page. |
| **More form options** | File uploads, autoresponders, Discord and Slack: [bootform.com/docs](https://bootform.com/docs/). |

### Stuck?

Open an [issue](https://github.com/BootForm/multi-step-form/issues) and say what happened.

---

## Frequently hit problems

**Clicking "Next" does nothing, or jumps past a step it shouldn't.** Check the browser console for
errors first. If you copied a `<div data-step>` block to add a step, make sure it still has the
`data-step` attribute (not just `data-step="2"` with a number the script doesn't look for; a bare
`data-step` is what the script actually selects) and, other than the very first one, that it
starts with `hidden`.

**"This form has reached its limit of 10 held submissions."** You tested it more than ten times
before claiming. Claim the form and the limit goes away.

## Licence

MIT. Do whatever you like with it, including using it for a real business.
