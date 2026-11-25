# Econ Job Scout

A Claude Code plugin for Erasmus University Rotterdam economics and business students, and anyone job-hunting in the Netherlands with an economics degree. Each person sets up and runs their **own** job scout:

- **Pick your fields.** Choose any mix of five field packs:

  | Pack | Tracks |
  | --- | --- |
  | **Finance** | Banking & M&A, private equity, asset management, trading, risk, corporate finance, FP&A, fintech |
  | **Consulting & strategy** | Strategy, management, economic and transaction consulting, sustainability consulting |
  | **Policy & public sector** | Central banking, government and regulators, economic research, international organisations |
  | **Data & analytics** | Business and data analytics, pricing, market research, quantitative analysis |
  | **Business & marketing** | Marketing and brand, commercial, supply chain, product, management traineeships |

- **A private tracker:** a claude.ai page with your pipeline, deadlines, daily finds and a watchlist of Dutch employers from your packs.
- **LinkedIn job alerts:** Claude generates the keywords that match your fields and tracks. You set them up as daily email alerts.
- **A daily scan:** every morning, Claude reads your LinkedIn alert emails (read-only). It checks each posting against *your* rules: field relevance, location, degree level, Dutch, timing and work permit. Roles that pass are added to your tracker.

The rules come from your CV and a few questions:
- **Degree:** if you'll never do a master's, master's-only roles are dropped.
- **Dutch:** if you speak fluent Dutch, Dutch-language roles stay in.
- **Work permit:** if you're an EU citizen, the permit check is skipped.
- **Fields:** marketing roles count only if you picked Business & marketing.

## What you need

- **The Claude desktop app** (the Code tab) or **Claude Code** in a terminal. The daily schedule needs the desktop app. In a terminal, you run the scan yourself or set it up with `/schedule`.
- **Git**, installed and on your PATH. Claude uses it to download the plugin from GitHub.
  - On Windows, install it from https://git-scm.com/download/win and keep the default options.
  - GitHub Desktop alone isn't enough, because its git isn't on the PATH.
  - Restart the Claude app after installing.
- **Your own Claude plan.** Everything runs on your own account.
- **The Gmail connector**, connected to the inbox that receives your LinkedIn alerts. In the app, go to Settings → Connectors → Gmail.
- **A LinkedIn account** for the job alerts. Setup gives you the exact list.
- **Your CV** (PDF or DOCX) in an empty folder, for example `Documents/job-scout/`.

## Install

First install Git (see above). Then use one of these:

- **Desktop app (Code tab):** click **+** next to the prompt box and choose **Add plugins**. Add the marketplace `nirmanyuatri-cloud/econ-job-scout`, then install **econ-job-scout** from it. The prompt box takes one line at a time, and typing `/plugin` there opens the plugins panel rather than running the command.
- **Claude Code in a terminal:** run these two lines one at a time:
  ```
  claude plugin marketplace add nirmanyuatri-cloud/econ-job-scout
  ```
  ```
  claude plugin install econ-job-scout@eur-econ-scout
  ```

Start a new session afterwards. Typing `/econ-job-scout:` should then show `setup` and `scan`.

To get updates, run `claude plugin marketplace update eur-econ-scout`.

**From a zip or folder:** unzip the repo somewhere permanent. Then run `claude plugin marketplace add ./path/to/econ-job-scout` and `claude plugin install econ-job-scout@eur-econ-scout`. To try it for one terminal session only, run `claude --plugin-dir ./path/to/econ-job-scout`.

## Set up (about 10 minutes)

1. Open a session in the folder that holds your CV.
2. Run `/econ-job-scout:setup`. Claude will:
   1. read your CV and ask only what it can't tell from it: your fields and tracks, master's plans, nationality and permit, Dutch level, start dates, and locations (optionally including Brussels, Frankfurt and Luxembourg);
   2. write `job-scout-profile.json` in your folder;
   3. publish **your own private tracker** and fill its watchlist from your packs;
   4. give you 3–4 combined LinkedIn alert searches and the click-by-click steps to create them (daily, by email);
   5. check your Gmail connector;
   6. create your daily scan at the time you choose.
3. Wait a day for the first alert emails. Then open the task under **Scheduled** in the sidebar and click **Run now**, and **watch this first run**. Claude asks for permission to use Gmail, the browser and your tracker. Choose **always allow** for each, so later runs can work on their own.

To change anything later, run `/econ-job-scout:setup` again in the same folder and choose **Update my setup**. That keeps your tracker and its history, refreshes the page design, and updates your alerts and schedule.

To preview without publishing or scheduling anything, run `/econ-job-scout:setup --dry-run`.

## Daily use

- New matches appear under **Daily finds** in your tracker. Each one shows the sentence from the posting that proves you're eligible.
- Change a role's status as you go: To apply, Applied, Interview, Offer.
- Roles marked **Unverified** (amber) couldn't be read in full, because LinkedIn rate-limits page loads. Check them before applying.
- Each scan adds at most 10 roles. Extra good roles are queued for the next days rather than dropped. It also checks 5 watchlist companies in rotation and re-checks a few older postings to mark the closed ones.
- If the app was closed for a few days, the next scan catches up on the alert emails it missed (up to 14 days).
- To change your rules or fields, re-run setup and choose **Update my setup**, or edit `job-scout-profile.json` (or ask Claude to). The next scan syncs the tracker's header and track list automatically.

## Privacy

- **Everything stays with you.**
  - Your CV and profile stay in your own folder.
  - Your tracker is a private artifact on your own claude.ai account. Nobody else can open it unless you share it.
  - Your scheduled task runs on your own computer and your own plan.
- **Gmail is read-only.** The scan only searches for and reads LinkedIn job-alert emails. It never sends, labels, archives or deletes mail. It stores only clean `linkedin.com/jobs/view/<id>` links, never the emails' tracking links, which carry personal tokens.
- **No data goes to the kit's authors.** The plugin is just instructions and templates. There's no server, no analytics and no shared database.
- **LinkedIn is browsed logged out, slowly**, with at most 8 page loads per scan. If LinkedIn rate-limits the scan, it stops using LinkedIn for that day and doesn't try to get around the limit.
- **Your CV stays local.** The scan uses the short CV summary in your profile file. Neither the CV nor the summary is copied into your tracker.

## What's in the kit

```
econ-job-scout/
├── .claude-plugin/
│   ├── plugin.json              plugin manifest
│   ├── marketplace.json         lets this repo act as its own marketplace
│   └── icon.png                 plugin icon
├── skills/
│   ├── setup/SKILL.md           /econ-job-scout:setup (onboarding)
│   └── scan/SKILL.md            /econ-job-scout:scan (daily scan logic)
├── packs/                       one file per field: tracks, keywords, relevance rules, watchlist
│   ├── finance.json
│   ├── consulting.json
│   ├── policy.json
│   ├── data.json
│   └── business.json
├── templates/
│   ├── tracker.html             the tracker page (personalises itself from your profile)
│   └── profile.example.json     profile schema with a fictional example
├── tests/gate-cases.md          sample postings with the expected outcome for two profiles
├── CHANGELOG.md
├── LICENSE                      MIT
└── README.md
```

## Adding or editing a field pack

Copy one of the files in `packs/` and keep the same keys: `id`, `label`, `tracks`, `groups` (at most 4), `baseKeywords`, `esgKeywords`, `dutchKeywords`, `adjacent`, `notRelevant` and `watchlist`. Then add the new pack to the field question in `skills/setup/SKILL.md`.

## For maintainers

- Validate before pushing: `claude plugin validate ./ --strict` and `claude plugin validate ./.claude-plugin/plugin.json --strict`.
- After editing the gates in `skills/scan/SKILL.md`, run through `tests/gate-cases.md` (see the instructions at the top of that file).
- Bump `version` in `plugin.json` whenever you change anything, so users get the update, and add a line to `CHANGELOG.md`.
- Never commit anyone's `job-scout-profile.json`, CV or tracker URL.
