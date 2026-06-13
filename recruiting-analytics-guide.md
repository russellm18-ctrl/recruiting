# Recruiting Subdomain Analytics — Reference Guide
**recruiting.russell-moore.com tracking, reporting, and athlete onboarding**

Last updated: June 12, 2026

---

## 1. What We Set Up (Overview)

`recruiting.russell-moore.com` is now fully wired into the same GA4 property as `www.russell-moore.com` (Measurement ID `G-WJW7ZYQK3Z`), tracked through Google Tag Manager container `GTM-MPGDW4QD`.

**Key IDs to remember:**
- GA4 Measurement ID: `G-WJW7ZYQK3Z`
- GTM Container ID: `GTM-MPGDW4QD`
- GA4 Property: `www.russell-moore.com` (same property covers both domains, distinguished by Hostname)

### What's tracking now

The GTM container has three tags, all live:

| Tag | Type | Fires On | Purpose |
|---|---|---|---|
| `Google Tag G-WJW7ZYQK3Z` | Google Tag (base config) | Initialization - All Pages | Loads GA4 connection |
| `Google Analytics GA4 Event` | GA4 Event (page_view) | All Pages | Records every page view |
| `GA4 Profile Link Click` | GA4 Event (`profile_link_click`) | All Link Clicks | Records every link click with destination URL + link text |

Plus **GA4 Enhanced Measurement** (built into the data stream, no GTM config needed) automatically tracks:
- Scrolls (90% depth)
- Outbound clicks (links to other domains — Hudl, YouTube, MaxPreps, etc.)
- Session start / engagement time

### Why this matters

Every page on `recruiting.russell-moore.com` — the sales page, every athlete profile — now reports:
- **How many times** it was viewed (Sessions, Active users, Views)
- **Where visitors came from** (Source/medium, Campaign, City)
- **What they did** (page_view, scroll, click events, engagement time, and specific link clicks)

---

## 2. Adding a New Athlete — Checklist

### A. Tracking (one-time setup, mostly automatic)

✅ **If the athlete page uses the shared recruiting site template/layout**, it already includes the `GTM-MPGDW4QD` snippet — tracking works automatically. No GTM changes needed per athlete.

⚠️ **If you ever build a page outside the shared template** (one-off page, different repo, etc.), confirm the GTM snippet is in the `<head>`:

```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-MPGDW4QD');</script>
<!-- End Google Tag Manager -->
```

And in `<body>`, immediately after the opening tag:

```html
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-MPGDW4QD"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
```

### B. Reporting — duplicate the Exploration template (~60 seconds)

1. In GA4, go to **Explore**
2. Open the **Athlete Traffic & Activity** exploration (the Ephraim Moore template)
3. Click the **⋮** menu on the exploration → **Make a copy**
4. Rename it with the new athlete's name (e.g., "Cohen Butler — Traffic & Activity")
5. In **both tabs** ("Traffic" and "Activity"), update the filter:
   - **Page path + query string** → change from `/ephraim-moore/` to the new athlete's slug (e.g., `/cohen-butler/`)
   - Leave the **Hostname** filter as `recruiting.russell-moore.com` — don't change this
6. Save

### C. Outreach link tagging (recommended, optional)

When sending an athlete's profile link to coaches, families, or posting it elsewhere, add UTM parameters so you can tell exactly where engagement came from:

| Scenario | Example URL parameters |
|---|---|
| Texted directly to a specific coach | `?utm_source=coach_outreach&utm_medium=text&utm_campaign=athletename` |
| Posted to Athletic.net / MaxPreps / similar | `?utm_source=athleticnet&utm_medium=referral&utm_campaign=athletename` |
| Family forwarding via email | `?utm_source=family&utm_medium=email&utm_campaign=athletename` |

These show up in the **Session campaign** and **Session source/medium** columns of the Traffic report.

---

## 3. How to Access the Data

### GA4 Property Navigation
- URL: `analytics.google.com` → property **www.russell-moore.com**
- Left sidebar icon (bar chart) → **Explore** → find your saved Explorations

### ⚠️ Important: Reporting Delay
- **Realtime** (Reports → Realtime) shows activity within ~30 seconds — use this for live demos (e.g., having a coach load the page while you watch)
- **Explorations / standard reports** have a **24–48 hour processing delay** — data from today won't appear in Explorations until tomorrow or later
- Always make sure your **date range** in the Exploration includes today/recent days (top-left date picker)

### Quick Realtime Check (for live demos)
1. Reports → Realtime → Realtime pages
2. Have someone load the athlete's page on their device
3. Watch "Active users in last 30 minutes" and the page path table update live
4. Realtime Overview → "Views by Page title and screen name" card confirms which specific page/title is getting traffic

---

## 4. Reading the Exploration — What Each Tab Tells You

### Tab 1: "Traffic" (Who came, where from)

| Column | What it tells you |
|---|---|
| Session source/medium | Direct, organic search, referral (e.g., Athletic.net), social, etc. |
| Session campaign | If UTM-tagged — tells you which specific outreach link was used |
| City | Geographic location of the visitor — useful for "a coach near [target school] viewed this" |
| Sessions | Total visits to the page |
| Active users | Unique people who visited |
| Avg. engagement time per session | How long they actually spent on the page (not just opened-and-bounced) |

### Tab 2: "Activity" (What they did)

| Event name | What it means |
|---|---|
| `page_view` | Page loaded |
| `scroll` | Visitor scrolled 90% down the page — they read through PRs/film/contact |
| `user_engagement` | Active engagement time recorded |
| `click` | Outbound click (Enhanced Measurement) — link to a different domain |
| `profile_link_click` | Any link click on the page, with `link_url` and `link_text` parameters — tells you exactly what they clicked (coach contact email, film link, schedule, etc.) |

To see **which specific link** was clicked, add **Link URL** or **Link Text** as a second row dimension under `profile_link_click` rows.

---

## 5. Turning This Into a Pitch Deliverable

### For an in-progress pitch / new prospect
Don't wait for retroactive data. Frame it forward-looking:
1. Build and launch the athlete's profile
2. Send the tagged link to coaches/programs immediately
3. **5–7 days later**, follow up with real numbers: "Here's what happened in the first week — X views, here's where they came from, here's how long coaches spent on the page, and here's what they clicked."

This follow-up email *with real data* is a stronger trust-builder than anything shown upfront.

### For a polished, shareable report (future enhancement)
A **Looker Studio** dashboard connected to this GA4 property, filtered per-athlete, can present:
- Scorecards: total views, unique visitors, avg. engagement time
- A simple map of visitor locations
- A table of link clicks (what they engaged with)

This can be shared as a link or screenshot — no GA4 login required for the parent/coach — and looks like a finished product deliverable rather than a raw analytics screen. Worth building once as a template, then duplicating per athlete (similar to the Exploration process above).

---

## 6. Troubleshooting Quick Reference

| Symptom | Likely cause | Fix |
|---|---|---|
| Exploration shows "No data" | 24–48hr processing delay, OR date range doesn't include recent days | Wait, and/or extend date range |
| New athlete page not appearing in reports | Page is missing the GTM-MPGDW4QD snippet | Check the page template includes the snippet (Section 2A) |
| "/" path shows combined traffic from multiple sites | Page path alone doesn't include hostname | Always filter/add **Hostname** dimension |
| Your own test visits not showing up after 48hrs | Internal Traffic filter may be excluding your IP | Check Admin → Data Settings → Data Filters |
| Realtime shows activity but Exploration doesn't (same day) | Normal processing delay | Check again tomorrow |

---

## 7. GTM Access Quick Reference

- GTM container: `tagmanager.google.com` → **Russell Moore Photography** → **www.russell-moore.com** → `GTM-MPGDW4QD`
- To verify what's live: **Versions** tab → top version = currently published
- To test changes before publishing: **Preview** button → opens Tag Assistant debugger in a new tab — check "Tags Fired" for each event
