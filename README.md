# Bryndis website — deploy bundle

Static site for **bryndis.com.au**. 5 pages, one CSS file, one `.htaccess`.
Built per AJ's Hostinger deploy rule: `index.html` + assets + `.htaccess`.

---

## Files

| File             | Purpose                                                              |
| ---------------- | -------------------------------------------------------------------- |
| `index.html`     | Home — hero, philosophy, features, HoH partnership, privacy promise. |
| `features.html`  | Deep dive on 4 core features + Date Buddy + Rideshare Tracker.       |
| `privacy.html`   | Privacy Policy. AU Privacy Act 1988 + 13 APP-aligned. Not lawyer-reviewed (disclaimer at bottom). |
| `terms.html`     | Terms of Service. AU Consumer Law aware. Not lawyer-reviewed (disclaimer at bottom). |
| `support.html`   | FAQ + 3 contact paths (hello / support / privacy).                   |
| `styles.css`     | Shared design system. ~750 lines. Forest + cream, Fraunces + Nunito. |
| `.htaccess`      | Force HTTPS, strip `www.`, pretty URLs, MIME types, security headers, cache. |

---

## ⚠️ PRE-DEPLOY CHECKLIST — do these BEFORE uploading

### 1. App Store URL (HARD BLOCKER)

Two occurrences of the placeholder `id0000000000` need to be replaced with the real App Store ID once Bryndis is live.

```bash
# Mac/Linux — preview
grep -n "id0000000000" *.html

# Replace (substitute the real numeric ID where 1234567890 is):
sed -i '' 's/id0000000000/id1234567890/g' index.html features.html
```

Found in: `index.html` line 292, `features.html` line 380.

**While the app is still TestFlight-only**, change those two `<a>` tags to point at a TestFlight invitation URL (`https://testflight.apple.com/join/XXXXXXXX`) and change the badge text to "Join the TestFlight beta".

### 2. Email mailboxes (HARD BLOCKER)

The site references these four addresses in `mailto:` links throughout. They must exist before launch — App Store review will email at least `privacy@` and `support@`.

- `hello@bryndis.com.au`
- `support@bryndis.com.au`
- `privacy@bryndis.com.au`
- `legal@bryndis.com.au` (only used in Terms)

Create these in Hostinger's email panel (or forward to your main inbox). The Privacy Policy promises a 30-day response window — set the forward up first, then deploy.

### 3. Subscription pricing (CONFIRM)

Terms of Service §6 and Features page reference **A$4.99/month or A$39.99/year** for paid modules. Confirm these match what's set up in App Store Connect, or change before publishing.

### 4. Postal address (SOFT)

Privacy Policy §13 and Support page both say *"email us first for the postal address"*. If you'd prefer to publish a PO Box or registered business address publicly, update:

- `privacy.html` — line 246 (`<strong>Post:</strong>` line)
- `support.html` — final paragraph above the bottom CTA

### 5. Legal review (RECOMMENDED, NOT BLOCKING)

Privacy Policy and Terms each end with a callout flagging *"has not been reviewed by a qualified Australian lawyer"*. Budget ~$300–$500 for a one-off review with LegalVision or similar. Replace the disclaimers and update the "Version 1.0" line once done.

### 6. RevenueCat dependency

Memory note: RevenueCat SDK key is revoked. Paid module CTAs on `features.html` link people to a subscription system that doesn't currently work. Either fix the RevenueCat key before going live, or temporarily hide the paid module sections (Date Buddy + Rideshare Tracker) with `display: none`.

---

## Deploy steps (Hostinger)

1. **File Manager** → navigate to `public_html/` (or the domain root for `bryndis.com.au`).
2. Upload all 7 files. Order doesn't matter, but keep them in the same directory.
3. Ensure `.htaccess` uploaded — it's often hidden in Mac Finder. Toggle hidden files (`Cmd+Shift+.`) before drag-and-drop, or use the Hostinger File Manager's upload button.
4. Verify in a private browser: `https://bryndis.com.au/` → 200, no mixed content.
5. Check redirects:
   - `http://bryndis.com.au/` → `https://bryndis.com.au/` (301)
   - `https://www.bryndis.com.au/` → `https://bryndis.com.au/` (301)
   - `https://bryndis.com.au/features.html` → `https://bryndis.com.au/features` (301)
6. Run Lighthouse against the live URL — should score 95+ across the board.

---

## Design system reference (for future edits)

- **Palette**: forest `#1F4332`, cream `#F5EFE4`, gold accent `#B8935A`. No red anywhere (deliberate, per Bryndis "bystander test").
- **Fonts**: Fraunces (variable, opsz + SOFT axis) for display + headings. Nunito for body.
- **Aesthetic**: editorial minimalist. Subtle paper-noise texture overlay. Generous whitespace. Numerals in Fraunces gold.
- **Motion**: hero phone tilts on hover; SOS circle pulses (3.2s ease-in-out); FAQ details disclose with `+` → `×` rotation. All animation respects `prefers-reduced-motion`.
- **CSS variables** are defined at the top of `styles.css`. Changing one value updates the whole site.

---

## SEO checklist (post-deploy)

- [ ] Submit `https://bryndis.com.au/sitemap.xml` to Google Search Console (generate when you have time — for 5 pages, a manual `sitemap.xml` is fine)
- [ ] Verify ownership in Search Console via DNS TXT record
- [ ] Add `robots.txt` allowing all (current omission means default-allow, which is fine for now)
- [ ] Confirm OG preview at https://www.opengraph.xyz/ — meta tags are in each `<head>` but OG image is not set; add `og:image` once you have a hero asset
- [ ] Bing Webmaster Tools (small bonus traffic)

---

## What's intentionally NOT in this build

- **Newsletter form** — no backend, no Formspree integration. Add later if needed.
- **Analytics** — no Google Analytics, no Plausible. Privacy Policy explicitly states this. Add Plausible if you want privacy-respecting analytics, and update §10 of the Privacy Policy.
- **Cookie banner** — not currently needed under Australian law (no advertising cookies, no third-party trackers). Revisit if Plausible is added.
- **Press kit / brand assets page** — defer until there's actual press interest.
- **Blog / changelog** — defer.

---

**Mode**: production. Built for direct ship to Hostinger.
