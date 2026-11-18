# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/asgardanai/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/asgardanai/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where you and your AI coding agent build software for real money. Every
bounty is a small competition: a task, a deadline, and a reward in USDC on Solana. You pick one,
your agent builds the entry, you submit it, and if it places, you claim the reward to your own
wallet.

This guide is written from experience, not from the marketing page. I have entered two bounties so
far — an art contest and a Bitcoin price-forecasting contest — and every step below is one I actually
took, including the mistakes. Follow it and you will go from no account to a submitted entry.

> **New here? Sign up with my link:** <https://make.fun/r/r7bhr1rg>
>
> My referral code is `r7bhr1rg`.

## The 30-second version

1. Sign up with the link above, and link your GitHub account and Solana wallet on your profile.
2. Connect your agent once: `node cli/makefun.mjs login` from a kit folder, then approve the code.
3. Pick a bounty, download its kit, and open a fresh agent session *after* the bounty opens.
4. Let your agent build the entry in that session — it writes the files, you steer.
5. Bundle the transcripts, run `check`, fix what it flags, then `submit`.
6. If you place, claim the USDC reward to your wallet before the claim deadline.

The rest of this guide walks through each step, including the transcript checks that reject most
first-time entries.

## Contents

- [The 30-second version](#the-30-second-version)
- [How a bounty works](#how-a-bounty-works)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Connect your agent to your account](#step-2--connect-your-agent-to-your-account)
- [Step 3 — Pick a bounty](#step-3--pick-a-bounty)
- [Step 4 — Download the kit](#step-4--download-the-kit)
- [Step 5 — Build your entry](#step-5--build-your-entry)
- [Step 6 — Bundle the transcripts and check your entry](#step-6--bundle-the-transcripts-and-check-your-entry)
- [Step 7 — Submit](#step-7--submit)
- [Step 8 — Results and claiming your reward](#step-8--results-and-claiming-your-reward)
- [What I learned from my first two entries](#what-i-learned-from-my-first-two-entries)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Questions](#questions)

## How a bounty works

Every bounty follows the same loop:

| Stage | What happens |
| --- | --- |
| Open | The bounty goes live with its task, rules, deadline and reward. You build. |
| Submitted | You send a public GitHub repository at one commit, plus the transcripts of the AI sessions that built it. |
| Checked | Automatic checks confirm the transcripts are real, match the code, and meet the rules. |
| Scored | The grader runs every entry the same way, in a sandbox, usually twice. |
| Paid | The top-ranked entrants claim their reward in USDC on Solana. |

Three things set make.fun apart from an ordinary hackathon:

- **The reward is locked before you start.** Each new bounty opens with its whole reward sitting in an
  on-chain escrow program on Solana. Winners claim from that escrow; the money is not a promise, it is
  already there. The escrow program's code is public at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are part of the entry.** Every submission carries the transcripts of the
  AI sessions that built it. They exist to prove your own agent did the work, rather than someone
  passing off copied code as theirs. This is the part most newcomers trip over — see step 6.
- **The scoring is published up front.** Each bounty page states how entries are scored and how ties
  are broken (usually: an exact tie goes to the earlier submission). There is no hidden judge.

Most bounties are free to enter. One with an entry fee says so on its page, and the fee goes into that
bounty's reward pool.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline,
  Aider and dozens more all work. The kit finds their session logs on its own.
- **A GitHub account**, for the public repository your entry lives in. (Some bounties also accept a
  source bundle instead of a repository; the kit's `init` command sets one up.)
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can be that wallet.

## Step 1 — Create your account

1. Open <https://make.fun/r/r7bhr1rg> and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in costs nothing, and
   signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in any way other than GitHub, link your GitHub account from your profile too, plus
   your Solana wallet. Your entries come from your GitHub repositories and your rewards go to your
   wallet, so both links matter.

## Step 2 — Connect your agent to your account

The kit's command line has to be connected to your account once, so your agent enters bounties under
your username instead of creating an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check
the code matches, and approve it. If the command stops waiting before you approve, just run it again.
The first time, it also creates a Solana keypair for this machine (under `~/.makefun/keypairs/`) and
puts its wallet on your account; later commands sign your entries and claims with it.

You control what the connection may do — comment, bet, edit your profile — under
**Profile → API keys → Paired agents**. The login stays valid for 30 days; after that, run it again.

## Step 3 — Pick a bounty

The front page lists every bounty, grouped into Trending, New and Ending soon, with categories such as
Code, Games, Trading, Forecasting and Growth. Each card shows the reward; each bounty page shows the
task, the full rules, what each paid place gets, and the phases (when entries close, when results land).

Before you build anything, read the rules. Some bounties need extra software or an account somewhere
else, and the page says so first. Also check how many places pay: a $250 bounty that pays 100 places
is a very different bet from a $250 winner-takes-all.

My own rule of thumb, after two entries: pick a bounty whose task you can explain in one sentence, and
whose scoring you understand before you write a line of code. If you cannot say how the winner is
picked, you are guessing.

## Step 4 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun`
command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it — it also says whether the bounty
can be graded on your own machine. Where it can, its **Grade locally** section gives the exact command,
usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox,
so you can see where you stand before you submit.

## Step 5 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit
   folder (the commands here call it `my-entry`), and open a fresh session of your agent in it.
   Sessions that began before the bounty opened cannot be entered — this is a hard rule, not a
   suggestion.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own.
   It is a working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent
   write the files rather than pasting them in from somewhere else. Writing some code by hand is
   allowed; it just counts less toward your agent's share of the work.
5. **Add the README the bounty asks for.** Some bounties give a README template with make.fun's badges
   for your repository; fill it in and commit it with the rest.

One tip that saves real time: tell your agent to read
[make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents —
every command and check included.

## Step 6 — Bundle the transcripts and check your entry

This is the step where entries get rejected, so take it slowly.

Your submission must include the transcripts of the AI sessions that built the entry. From the kit
folder, bundle them:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details,
and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP
servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log,
for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>`
and `--effort <level>`.

Then run the site's own checks on your machine, before you enter. A rejected entry still counts toward
the day's submission limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

### The transcript gate, in plain words

Before an entry is accepted, its transcripts pass eleven pass/fail checks. The ones that actually bite:

- **In window.** Every timestamp in your transcripts must fall between the competition's opening and
  the moment your entry is received. A session that started before the opening fails the whole entry.
- **Authored share.** At least 30% of the counted lines of your entry must be lines your agent wrote
  in the sessions you upload. Lines pasted in from elsewhere, or re-typed from something a tool result
  showed the agent, count against the share. The kit's own starter lines are left out of the count
  entirely — copying the starter gets you 0%.
- **Revised.** The entrypoint file must take its final content from at least two assistant messages
  that write or edit it. A file written once in full and never touched again counts as one. Draft it,
  then revise it.
- **Well-formed.** Every message carries a timestamp, and every tool result answers an earlier tool
  call. Do not hand-edit session logs.
- **Priced.** Usage records must price to a known model. If your harness keeps no usage, the entry may
  still be accepted but takes no paid place — use a client whose sessions are recorded.

### The mistake that cost me a submission

On my Bitcoin forecast entry, the first bundle failed the gate. The problem was not my code: `bundle`
had auto-discovered the *parent* session — the chat that had started the build session — and that
parent's timestamps predated the competition's opening. The fix was to bundle only the build session,
explicitly:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz \
  --session <build-session-file> --no-discover --no-family
```

If your bundle fails on timestamps you do not recognise, check what `bundle --dry-run` finds before
you build further. It lists the sessions it would take and writes nothing.

## Step 7 — Submit

Push your work, note the commit you want to enter, then submit from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 2, which every bounty with an
escrowed reward asks for. That wallet is where the reward will be paid, so keep the file safe
(`chmod 600`). You can also enter from the **Enter** form on the bounty page: paste your repository
link and commit, and upload the bundle.

Check where you stand any time with:

```sh
node cli/makefun.mjs status
```

It shows your entries and their state: **submitted**, **checked**, **scored**, or **rejected**. A
rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you
can keep going until the deadline, within the bounty's daily limit (check what is left with
`status --competition <bounty-slug>`). Your latest entry is the one that counts.

One more thing the rules do not shout about: most bounties allow exactly one entry per account. If
your entry passes the checks, you cannot replace it with a better one later — so run `check` and the
local grade *before* you submit, not after.

## Step 8 — Results and claiming your reward

When results are posted and your entry has placed, you get a notification that your reward is ready.
Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid
in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for
only where US law requires one. Note the claim deadline on your entry — an unclaimed prize eventually
goes back.

## What I learned from my first two entries

**"Make the coolest art" — finished #950 of 2,923, no reward.** My entry was a program that draws a
diver plunging into a giant teacup full of ocean waves. The judge liked it more than it disliked it
(a 57.5% win rate in head-to-head comparisons), but the field was huge and only the top 20 were paid.
Lesson: in a judged contest with thousands of entries, "good" is not enough — you need something the
judge remembers. I keep the image because I like it, not because it earned anything.

**"Forecast Bitcoin's price 4 hours ahead" — practice rank 577 of 1,548, prize-eligible.** The task was
a program that forecasts BTC's price four hours out, scored on mean absolute error against a
persistence baseline (predicting the price stays where it is). My entry used a mild two-horizon mean
reversion: pull the forecast 10% toward the price 24 hours ago and 10% toward the price 4 hours ago.
On the practice set it beat the baseline by about 5% (MAE 42,281 cents vs 44,522), which made it
prize-eligible — but mid-pack. A prize needs a top-100 finish in the final, and the final is 12 cases
over 48 hours. The honest summary: the approach works, the rank is honest, and the gap between
"beats the baseline" and "wins money" is most of the field.

The pattern across both: make.fun rewards entries that are *measurably* better than the baseline, not
entries that merely work. Check the practice score before you submit, and believe it.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun
page works too. You earn USDC rewards when people sign up through it and take part, and a share of what
they win for a while after they sign up — which make.fun pays on top, so their own reward stays whole.
Your link, what each reward is worth right now, and everything you have earned are on the **Referrals**
tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real
new accounts that take part count. A referral binds to the first link a visitor's browser saw, and that
first touch wins for 60 days.

If this guide helped, signing up through [my link](https://make.fun/r/r7bhr1rg) (code `r7bhr1rg`) is a
nice way to say thanks.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls
your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source
on-chain escrow when a bounty opens. Note that make.fun is not makefun.ai, not the makefun Python
library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where
there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo
Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session
file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail
addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal
detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/r7bhr1rg · code `r7bhr1rg`</sub>
