# CBT Purchase Funnel — Experience Audit

A running log of conversion pitfalls and opportunities across the CBT Purchase Funnel,
built frame-by-frame from Figma using the [checkout-cro-audit](https://github.com/callmedell/checkout-cro-audit-skill)
Cursor skill.

**Live deck:** https://callmedell.github.io/cbt-purchase-funnel-audit/
(password-protected — ask Wendell for the access code)

## What's in this repo

| Path | Purpose |
|---|---|
| `checkout-cro-deck.html` | The deck itself — one self-contained HTML file, no build step. Open it directly or view it via GitHub Pages. |
| `index.html` | Redirects the root Pages URL to `checkout-cro-deck.html`. |
| `assets/` | Screenshots of each audited Figma frame, downloaded locally since Figma's asset URLs expire quickly. |

## Structure

Each frame (or small set of frames representing one flow) reviewed adds a "frame group"
to the deck: an Overview slide (screenshot + context), a Pitfalls slide, and an
Opportunities slide (impact vs. effort). Some groups also have a bonus slide — page-level
thoughts, a bigger look at one specific element, or a Jobs-To-Be-Done breakdown for a
shared component.

Navigate with the arrow keys, the on-screen prev/next buttons, or the "Jump to frame"
dropdown (bottom-left) to skip straight to any frame group's Overview slide.

## Updating

New frame groups are added by running the `checkout-cro-audit` skill in Cursor with a
Figma frame link and some context. After editing, publish changes with:

```bash
cd ~/cro-audit-deck
git add -A && git commit -m "Add frame group N: <name>" && git push
```

GitHub Pages redeploys automatically a short time after each push.
