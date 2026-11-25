# Finance Job Scout

A Claude Code plugin for Erasmus University Rotterdam finance students (and anyone job-hunting in Dutch finance). Each person sets up and runs their **own** job scout:

- **A private tracker:** a claude.ai page with your pipeline, deadlines, daily finds and a watchlist of about 20 Dutch finance employers.
- **LinkedIn job alerts:** Claude generates the keywords that match your interests, and you set them up as daily email alerts.
- **A daily scan:** every morning Claude reads your LinkedIn alert emails (read-only) and checks each posting against *your* rules: location, degree level, Dutch, timing and work permit. Roles that pass are added to your tracker.

The rules come from your CV and a few questions. If you'll never do a master's, master's-only roles are dropped. If you speak fluent Dutch, Dutch-language roles stay in. If you're an EU citizen, the permit check is skipped. And so on.

## What you need

- **The Claude desktop app** (the Code tab) or **Claude Code** in a terminal. The daily schedule needs the desktop app. In a terminal you run the scan yourself or set it up with `/schedule`.
- **Git**, installed and on your PATH. Claude uses it to download the plugin from GitHub. On Windows, install it from https://git-scm.com/download/win and keep the default options. GitHub Desktop alone isn't enough, because its git isn't on the PATH. Restart the Claude app after installing.
- **Your own Claude plan.** Everything runs on your own account.
- **The Gmail connector**, connected to the inbox that receives your LinkedIn alerts. In the app, go to Settings → Connectors → Gmail.
- **A LinkedIn account** for the job alerts. Setup gives you the exact list.
- **Your CV** (PDF or DOCX) in an empty folder, for example `Documents/job-scout/`.

## Install

### Option A: from GitHub (recommended)

First install Git (see above). Then use one of these:

- **Desktop app (Code tab):** click **+** next to the prompt box, choose **Add plugins**, add the marketplace `nirmanyuatri-cloud/finance-job-scout`, and install **finance-job-scout** from it. The prompt box takes one line at a time, and typing `/plugin` there opens the plugins panel rather than running the command.
- **Claude Code in a terminal:** run these two lines one at a time:
  ```
  claude plugin marketplace add nirmanyuatri-cloud/finance-job-scout
  ```
  ```
  claude plugin install finance-job-scout@eur-finance-scout
  ```

Start a new session afterwards. Typing `/finance-job-scout:` should then show `setup` and `scan`.

Updates: `/plugin marketplace update eur-finance-scout`.

### Option B: from a zip or folder

1. Unzip `finance-job-scout.zip` somewhere permanent, for example `Documents/plugins/finance-job-scout`.
2. In Claude Code, add it as a local marketplace and install:
   ```
   /plugin marketplace add ./path/to/finance-job-scout
   /plugin install finance-job-scout@eur-finance-scout
   ```
   Use a path starting with `./` or `../`, or an absolute path.
3. To try it for one terminal session only, run `claude --plugin-dir ./path/to/finance-job-scout` instead.

## Set up (about 10 minutes)

1. Open a session in the folder that holds your CV.
2. Run `/finance-job-scout:setup`. Claude will:
   1. read your CV and ask only what it can't tell from it (master's plans, nationality and permit, Dutch level, start dates, interests, locations);
   2. write `job-scout-profile.json` in your folder;
   3. publish **your own private tracker** and fill in its watchlist;
   4. give you your LinkedIn alert keywords and the click-by-click steps to create them (daily, Netherlands, by email);
   5. check your Gmail connector;
   6. create your daily scan at the time you choose.
3. Wait a day for the first alert emails. Then open the task under **Scheduled** in the sidebar and click **Run now**, or type `/finance-job-scout:scan`.

To preview without publishing or scheduling anything, run `/finance-job-scout:setup --dry-run`.

## Daily use

- New matches appear under **Daily finds** in your tracker, each with the sentence from the posting that proves you're eligible.
- Change a role's status as you go: To apply, Applied, Interview, Offer.
- Roles marked **Unverified** (amber) couldn't be read in full because LinkedIn rate-limits page loads. Check them before applying.
- To change your rules, edit `job-scout-profile.json`, or ask Claude to (for example, "I now speak B1 Dutch"). The scan reads it on every run. Rerun setup step 5 ("re-seed my tracker config") so the tracker's header text updates too.

## Privacy

- **Everything stays with you.** Your CV and profile stay in your own folder. Your tracker is a private artifact on your own claude.ai account, and nobody else can open it unless you share it. Your scheduled task runs on your own computer and your own plan.
- **Gmail is read-only.** The scan only searches for and reads LinkedIn job-alert emails. It never sends, labels, archives or deletes mail. It stores only clean `linkedin.com/jobs/view/<id>` links, never the tracking links in the emails, which carry personal tokens.
- **No data goes to the kit's authors.** The plugin is just instructions and templates. It has no server, no analytics and no shared database.
- LinkedIn is browsed logged out and paced slowly. If LinkedIn rate-limits the scan, it stops using LinkedIn for that day and does not try to get around the limit.

## What's in the kit

```
finance-job-scout/
├── .claude-plugin/
│   ├── plugin.json              plugin manifest
│   └── marketplace.json         lets this repo act as its own marketplace
├── skills/
│   ├── setup/SKILL.md           /finance-job-scout:setup (onboarding)
│   └── scan/SKILL.md            /finance-job-scout:scan (daily scan logic)
├── templates/
│   ├── tracker.html             the tracker page (personalises itself from your profile)
│   ├── profile.example.json     profile schema with a fictional example
│   └── watchlist-eur-finance.json  default watchlist of Dutch finance employers
└── README.md
```

## For maintainers

- Validate before pushing: `claude plugin validate ./ --strict` and `claude plugin validate ./.claude-plugin/plugin.json --strict`.
- Bump `version` in `plugin.json` when you change anything, so users get the update.
- Never commit anyone's `job-scout-profile.json`, CV or tracker URL.
