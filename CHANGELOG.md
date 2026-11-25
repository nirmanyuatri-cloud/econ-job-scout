# Changelog

## 0.3.0
- **Scan remembers what it screened.** Job IDs it has already checked, including rejected ones, are stored in each day's scan, and it never opens them again.
- **Catches up after missed days.** It searches Gmail from the last scan date (up to 14 days back) instead of a fixed 2 days.
- **LinkedIn page budget.** Each run makes at most 8 LinkedIn page loads: alert postings first, then the watchlist, then closed-posting checks.
- **Watchlist rotation.** It checks 5 companies per day, oldest first, trying careers pages before LinkedIn.
- **Closed-posting checks** cover at most 5 rows per day, only rows at least a week old, and record `lastVerified`.
- **Overflow queue.** Roles that pass but go over the 10-a-day cap are queued for the next days instead of being dropped.
- **CV summary.** Setup writes `cvSummary`, so the scan no longer re-reads the CV every day.
- **Profile sync.** The scan updates the tracker's `config/profile` when the profile file changes.
- **Optional EU hubs** (Brussels, Frankfurt, Luxembourg) via `extraLocations`, with the right permit and language rules.
- **Setup update mode.** Re-running setup keeps the same tracker, refreshes its page design, adds new watchlist companies only, and updates the scheduled task instead of creating a second one.
- **Combined LinkedIn alerts.** Up to 4 OR-searches instead of 15 separate alerts, with a fallback.
- **Watching the first run.** Setup tells users to watch their first run and choose "always allow", so later scheduled runs don't stall.
- **Unattended runs.** The scan never stops to ask questions mid-run.
- **Tracker links.** The tracker only links `https://` URLs.
- **Careers links** in all packs are checked; 9 dead and 5 redirected links are fixed.
- **Repo files.** Added a LICENSE, an icon, this changelog and `tests/gate-cases.md`.

## 0.2.0
- Generalised from Finance Job Scout to Econ Job Scout, with five field packs: finance, consulting, policy, data and business.

## 0.1.x
- First version: the finance-only scout, with tracker, setup and scan.
