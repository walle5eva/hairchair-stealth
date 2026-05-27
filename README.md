# hairchairos.com — public stealth site

Single-file static site for HairChair, served from GitHub Pages at **hairchairos.com**. Drop these files into the repo, push to `main`, and the site is live.

---

## Files in this drop

| File | Purpose | Edit how often |
|---|---|---|
| `index.html` | The entire site. HTML + inline CSS + zero JS. | Whenever phases ship or copy changes |
| `CNAME` | Tells GitHub Pages to serve at `hairchairos.com`. **Required.** Do not delete. | Never |
| `favicon.svg` | Browser tab icon. Copper "h" monogram with the OS green dot. | Only if you redo the brand mark |
| `README.md` | This file. Internal. Doesn't ship to the visitor. | When deployment steps change |

---

## 1. First-time deploy (you've probably already done this)

If you haven't yet created the repo that GitHub Pages serves from:

```bash
# Pick a name. The conventional one is <username>.github.io for a user-level site,
# or a project repo (e.g. "hairchair-stealth") for a project-level site.
# Either works with a custom domain. The CNAME file does the routing.

git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# Drop these four files in the root of the repo
cp /path/to/index.html .
cp /path/to/CNAME .
cp /path/to/favicon.svg .
cp /path/to/README.md .

git add .
git commit -m "Initial public site for hairchairos.com"
git push origin main
```

Then in GitHub:

1. Repo **Settings → Pages**
2. **Source:** Deploy from a branch → `main` → `/ (root)` → Save
3. **Custom domain:** `hairchairos.com` (GitHub reads the `CNAME` file but confirm here too)
4. **Enforce HTTPS:** wait ~10 minutes for GitHub to issue the cert, then check the box

Namecheap DNS should have these four `A` records pointing to GitHub's IPs (which you've already set per Gemini's instructions):

```
@   A   185.199.108.153
@   A   185.199.109.153
@   A   185.199.110.153
@   A   185.199.111.153
```

Plus a `CNAME` record from `www` → `<your-username>.github.io` so `www.hairchairos.com` also resolves.

---

## 2. Updating phase status as the sprint progresses

The phase timeline in section 04 is the part of the site that will change most. Every phase has a `<div class="phase-status">` with one of three CSS classes:

| Class | Meaning | Visual |
|---|---|---|
| `status-done` | Gate cleared, phase shipped | Emerald dot, "Complete" |
| `status-active` | Currently in progress | Glowing copper dot, "In Progress" |
| `status-queued` | Upcoming | Hollow dot, "Queued" |

To mark a phase done and advance the next one, change two blocks:

```html
<!-- Outgoing phase → Done -->
<div class="phase-status status-done"><span class="dot"></span>Complete</div>

<!-- Incoming phase → Active -->
<div class="phase-status status-active"><span class="dot"></span>In Progress</div>
```

That's the entire update. Commit, push, the site reflects reality within seconds of GitHub Pages rebuilding (usually under a minute).

You can also rewrite the `<p class="phase-desc">` text once the phase is complete to describe what actually shipped rather than what was planned. That makes the site a real build log over time, not a static announcement.

### The weekly "Now Shipping" callout

Above the phase timeline sits the **Now Shipping** block. Update this every Sunday night or Monday morning with what you're actively working on **that week**. It's the part of the site that signals "this is a live build, not a press release."

Find this block in `index.html` (search for `<!-- NOW SHIPPING -->`) and edit two things:

1. The label: `Now Shipping · Week N of Phase M`
2. The sentence below: 2–3 short clauses naming the specific work. Use `<em>` tags around proper nouns (tools, libraries, hardware) to get the copper italic accent.

Example for Week 2 of Phase 1:

```html
<span class="now-shipping-label">Now Shipping · Week 2 of Phase 1</span>
<p>
  Tuning <em>OmniHair 4C</em> physics on the mannequin head. Building the parameter-control Kit extension.
  Saving the first <em>Hair Preset Library</em> JSONs for 4C, 4B, 4A, and 3C.
</p>
```

If you're between phases or on break, you can also remove the block entirely. It's optional — the timeline below still works without it.

---

## 3. Swapping the contact email

The site uses `ej@hairchairos.com` in two places: the waitlist form's "or write directly" fallback and the footer link. If you set up your inbox under a different handle, find-and-replace those two strings in `index.html`. There are no other places the address appears.

---

## 4. Adding the founder portrait

The `From the Founder` section has a placeholder frame that currently displays an "EJG" monogram as a graceful fallback. To use a real photo:

1. Add `assets/founder.jpg` to the repo (4:5 portrait orientation works best, ~800×1000 px)
2. In `index.html`, find the comment that begins `<!-- Drop a square portrait...`
3. Uncomment the `<img>` tag directly below it

The CSS already handles cropping and sizing.

---

## 5. Adding Open Graph and social preview images

When the LinkedIn or X preview card pulls from `hairchairos.com`, it currently points at `/assets/og-cover.jpg` which does not yet exist. Drop a 1200×630 image at that path and the social preview will render correctly. Until then, the preview shows the title and description only — still clean, just no hero image.

A good first OG image: a clean hardware render of the chair on a neutral background, with the wordmark in the corner.

---

## 6. Brand tokens (for future edits)

All colors and fonts are defined as CSS custom properties at the very top of the `<style>` block. To shift the palette site-wide, edit only those values:

```css
--ink-deep:    #0a0e16;   /* primary background */
--ink-raised:  #111827;   /* cards */
--ink-rule:    #1f2937;   /* hairline borders */

--bone:        #f4eee2;   /* warm off-white text */
--bone-dim:    #c9c2b3;   /* secondary */
--bone-faint:  #8c8678;   /* labels / metadata */

--copper:      #d0876a;   /* hardware heat accent */
--emerald:     #4ade80;   /* OS / software accent */
--signal-blue: #60a5fa;   /* future / queued accent */
```

The typography uses **Fraunces** (variable serif, optical sizing) for display, **Manrope** for body, **JetBrains Mono** for technical accents. All loaded from Google Fonts with `display=swap` so text never blocks rendering.

---

## 7. What this site deliberately does NOT do

- No analytics, no tracking, no third-party scripts. Privacy-clean. Add later if you want Plausible or simple Cloudflare analytics — both are GDPR-safe.
- No newsletter signup. Email is the channel.
- No live demo. Phase 6 will likely warrant a separate demo video page.
- No investor deck link in public. That stays under NDA.
- No team page. You are sole founder.
- No claims that could trigger FDA. Every word of copy was written to honor the General Wellness positioning in `regulatory_extractions.md`. Do not add "diagnose," "treat," "therapeutic," "medical," "cure," or "prevent." Use "insights," "assessment," "support," "general wellness," "haircare." Run the language audit before any future copy change ships.

---

## 8. Verifying the site is live

After pushing:

```bash
# Check HTTPS resolves and returns a 200
curl -I https://hairchairos.com

# Should see something like:
# HTTP/2 200
# server: GitHub.com
# content-type: text/html; charset=utf-8
```

Then open `https://hairchairos.com` in an incognito window. If you see anything stale, clear the DNS cache. On macOS:

```bash
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

---

## 9. Next site evolution candidates (not for v1)

These are reasonable additions but explicitly out of scope right now. Each is a paragraph of effort, not a project. Listed in rough priority order:

1. **A `/build` route as a real build log** with dated entries per phase, technical photos, and short notes. Convert from single-file to Jekyll when this becomes worth the maintenance.
2. **A `/press` route** once you have a notable mention.
3. **A `/research` route** linking to the master's thesis once it's defended.
4. **A `/careers` route** when you start hiring. Not before.
5. **Light/dark toggle.** The brand is dark-mode-first and that's a feature, not a limitation. Only add if a real user requests it.
