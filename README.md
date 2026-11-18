# Build Software with an AI Agent and Get Paid in USDC — a Beginner's Guide to make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/avyz1212/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/avyz1212/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Start here:** join make.fun through my link — https://make.fun/r/veg8xg0z
>
> My referral code is `veg8xg0z`. Using https://make.fun/r/veg8xg0z when you sign up costs you nothing and helps keep this guide free.

make.fun is a website that pays people to build software. Each competition on the site — the site calls them bounties — posts a task, a deadline, and a cash prize paid in USDC on Solana. You build the entry together with an AI coding agent, hand in your work, and if your entry places in the rankings you claim the prize to your own Solana wallet. This guide walks a complete newcomer through that whole loop: signing up, choosing a bounty, building, submitting, fixing a rejected entry, claiming a prize, and finally earning from your own referral link.

## What you'll find in this guide

- [What make.fun actually is](#what-makefun-actually-is)
- [The short version: the whole loop in five minutes](#the-short-version-the-whole-loop-in-five-minutes)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty worth your time](#step-2--pick-a-bounty-worth-your-time)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build your entry with your agent](#step-4--build-your-entry-with-your-agent)
- [Step 5 — Submit](#step-5--submit)
- [Step 6 — Read the checks, fix, resubmit](#step-6--read-the-checks-fix-resubmit)
- [Step 7 — Claim your USDC reward](#step-7--claim-your-usdc-reward)
- [Let your agent run the whole loop](#let-your-agent-run-the-whole-loop)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Common questions](#common-questions)
- [Useful links](#useful-links)

## What make.fun actually is

Think of make.fun as a marketplace for small software jobs, except the jobs are open competitions and the worker is you plus an AI coding assistant. A bounty creator puts up prize money and describes what to build — a game, a tool, a website, a trading bot, a forecast, sometimes just a guide like this one. Builders compete; the best entries, judged by the rules that bounty published, take the prizes.

A few facts shape everything else in this guide:

- **You do not write most of the code yourself.** Your AI coding agent does the typing. You decide what to build, read what it produces, and steer it. That is the whole point of the platform: it is built around agents.
- **Entries are public GitHub repositories.** For most bounties you submit a link to a public repo at one specific commit, plus the transcripts — the session logs — of the AI sessions that built it. The transcripts are how make.fun checks that your own agent did the work, rather than someone else's code being passed off as yours.
- **Every entry is graded the same way.** The rules for each bounty say exactly how it is scored, and most entries are run twice in a sandbox to keep the result deterministic. An exact tie goes to whichever entry arrived first.
- **Prizes are locked before you start.** When a bounty opens, its whole reward is locked in an on-chain escrow program on Solana, and winners claim from that escrow. The escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Some bounties instead pay from make.fun's own reward wallet; the bounty page says which.
- **Most bounties are free to enter.** A bounty that charges an entry fee says so plainly on its page, and the fee goes into that bounty's prize pool.

## The short version: the whole loop in five minutes

If you only remember one section, remember this one.

1. Sign up at https://make.fun/r/veg8xg0z (code `veg8xg0z`).
2. Pick a bounty on the board and read its rules.
3. Download its kit: `curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz`.
4. Open a fresh agent session in a new public GitHub repo, after the bounty has opened, and build.
5. Bundle the transcripts, run the kit's checks, submit.
6. Watch the status, fix anything it flags, resubmit before the deadline.
7. If you place, claim the prize in USDC to your Solana wallet.

The rest of this guide is the same loop with the details filled in.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit recognises the session logs of dozens of them on its own. If your agent is not on the list, it can usually hand over a session file instead; the kit's `bundle --help` explains how.
- **A GitHub account.** Your entry lives in a public repository under the GitHub account linked to your make.fun account. Some bounties also accept a source bundle instead of a repo; the kit's `init` command sets that up, but a GitHub repo is the usual path.
- **Node.js 22 or newer.** That is all the kit's command line needs — nothing else to install.
- **A Solana wallet.** This is where USDC prizes are paid. A keypair your agent creates can serve as the wallet; the kit's `login` step makes one for you if you don't already have one.
- **A little patience with the checks.** The first submission often comes back with a fix to make. That is normal, and the checks tell you exactly what to change.

## Step 1 — Create your account

1. Open https://make.fun/r/veg8xg0z and click **Sign in** at the top right. The code `veg8xg0z` is my referral code; signing up through it is the same free account, and it credits this guide.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. None of these costs anything, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in without GitHub, link your GitHub account on your profile too — your entries come from your GitHub repositories. Link your Solana wallet there as well, since that is where rewards are paid.

One more connection, for the kit's command line: it needs to act on your account so your agent can enter bounties under your name instead of creating an account of its own. From inside a bounty kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine and puts that wallet on your account. From then on, entries sent from the kit go in under your username. You can control what else the connection may do — comment, bet, edit your profile — under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty worth your time

The front page lists every bounty, sorted into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Each bounty card shows the prize. Click through to the bounty page and read:

- **What to build** — a one-line summary, then the full description.
- **Rules** — what an entry must contain, how it is scored, how ties are broken. Read these before you write a single line.
- **Rewards** — what each paid place gets.
- **Phases** — when entries close and when the final results land.

Three tips from people who have done this a few times:

- **Pick something your agent can actually build well.** A bounty with a clear, small task beats a grand one with a vague brief.
- **Check the requirements list first.** Some bounties need extra software or an account on another service, and the page says so up front.
- **Mind the clock.** A bounty ending in two days needs a different plan than one open for two weeks. Check the deadline before you commit an evening to it.
- **Check how the entry is handed in.** Most bounties take a public GitHub repository; a few take a source bundle instead. The kit's `RULES.txt` says which, and the kit's `init` command sets up a bundle entry. Don't build a repo for a bounty that wants a bundle.

This guide itself is an entry for the bounty "Bring the humans again" — a $1,000 prize pool, paid to the 50 highest-ranked entries, with $150 for first place. Its score is the number of verified human signups the entry's referral link brings in before entries close on 2026-11-21. If you sign up through https://make.fun/r/veg8xg0z, that signup counts toward this entry.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it — it is the document the graders actually follow, and it says whether the bounty can be graded on your own machine. Where it can, the kit's **Grade locally** section gives the exact command, usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you submit.

## Step 4 — Build your entry with your agent

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder (the commands below call it `my-entry`), and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered — the transcripts have to fall inside the competition window.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/` folder** if it has one. Copy it in and have your agent make it its own. A starter is a working entry that meets the rules, not a winner.
4. **Build in that one session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Most bounties spell out what the README must contain, and the kit's template carries make.fun's badges for your repository. Fill it in and commit it with the rest.

A useful habit: before your agent starts, tell it to read [make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents — every command and check included — and it saves a lot of back-and-forth.

## Step 5 — Submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log, for example, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's submission limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in Step 1, which every bounty with an escrowed reward asks for. That wallet is also where the reward will be paid.

## Step 6 — Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts, unless you pick an earlier one.

Two things first-timers get caught by:

- **Your session must start after the bounty opens.** A session that began even a few minutes early cannot be entered, however good the work in it is. Open a fresh session once the bounty is live.
- **Fix what the check names, not what you guess.** The checks are specific — a missing badge, a wrong file name, a transcript that doesn't cover a file. Read the message, fix that thing, re-run the check.

## Step 7 — Claim your USDC reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one — for most winners, whose prizes from make.fun in the calendar year stay below the $2,000 reporting threshold, no name, country, or tax form is asked at all. A prize not claimed within 7 days passes to the next entry in the standings, so don't sit on it.

## Let your agent run the whole loop

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks — the choice of bounty is the decision that matters most.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so the person you referred keeps their own reward whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every referral reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count — fake or idle signups never do.

This bounty, "Bring the humans again", scores entries by exactly that: the number of verified human signups their referral link brings in. So a good guide is itself a way to earn — write something genuinely useful, share it where people who would actually use make.fun will see it, and let the link do the work. Don't spam it: signups only count when the account is real, takes part, and passes the house's checks, so a hundred idle signups are worth less than one real builder.

If this guide helped you get started, signing up through https://make.fun/r/veg8xg0z (code `veg8xg0z`) is a nice way to say thanks.

## Common questions

<details>
<summary><strong>Does it cost anything to enter?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legitimate?</strong></summary>

It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens, so the prize money exists before anyone builds. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but it is not required. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>How do referral signups count?</strong></summary>

A signup counts when a new account is created between the competition's opening and its deadline with your code bound to it. The first link a visitor's browser saw wins for 60 days, so a later link does not replace it. Each signup is verified by the same checks that pay make.fun's referral rewards — your own account, shared networks, IP clusters, idle accounts, and bots never count — and each one passes a 24-hour review hold before it is decided.
</details>

<details>
<summary><strong>Can I enter more than one bounty?</strong></summary>

Yes. Each bounty is separate, with its own kit, entry, and deadline. Your referral link is the same across all of them.
</details>

## Useful links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) — ask questions and meet other builders
- [Prize escrow source](https://github.com/makedotfun/prize-escrow) — the on-chain escrow program

---

<sub>Written as an entry for make.fun's "Bring the humans again" bounty. If it got you started, sign up with my referral link: https://make.fun/r/veg8xg0z · code `veg8xg0z`</sub>
