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

## Visitor comments (Supabase setup)

The deck has a comment icon and an activity (clock) icon fixed in the top-right corner.
Visitors can suggest a Pitfall, Opportunity, or Question to Ask for whichever frame group
they're currently viewing; submissions are auto-injected into that group's matching slide
(marked with a "Visitor suggestion" badge) and listed in the activity flyout with a
timestamp.

This requires a free Supabase project to actually persist and share comments across
visitors (without it, the UI still works but comments only live in the current browser
tab and vanish on reload).

**One-time setup:**

1. Sign up / log in at [supabase.com](https://supabase.com) and create a new project
   (any name/region; the database password doesn't matter for this use case).
2. Open the SQL Editor and run:

   ```sql
   create table public.comments (
     id uuid primary key default gen_random_uuid(),
     created_at timestamptz not null default now(),
     frame_group text not null,
     category text not null check (category in ('pitfall', 'opportunity', 'question')),
     comment text not null,
     author_name text
   );

   alter table public.comments enable row level security;

   create policy "Allow public insert" on public.comments
     for insert to anon
     with check (true);

   create policy "Allow public read" on public.comments
     for select to anon
     using (true);
   ```

3. Go to **Project Settings → API** and copy the **Project URL** and the **anon public**
   key.
4. In `checkout-cro-deck.html`, near the bottom, replace:

   ```js
   var SUPABASE_URL = 'YOUR_PROJECT_URL';
   var SUPABASE_ANON_KEY = 'YOUR_ANON_KEY';
   ```

   with the real values, then commit and push.

**Known limitation:** the deck's password gate is client-side only (a soft deterrent, not
real access control — same as the Pie-Pattern deck). The Supabase anon key is also
necessarily public (embedded in the page), so its RLS policies are intentionally scoped
to insert + read only — no update/delete policies exist, so visitors can add comments but
never alter or remove existing ones. Anyone with the Supabase URL could theoretically hit
the API directly, bypassing the page's password gate — acceptable for an internal-review
tool, not for anything sensitive.
