# Your first make.fun bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/gullalder/bring-the-humans-02/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/gullalder/bring-the-humans-02/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

**[Create your make.fun account](https://make.fun/r/r1te5ect)** for AI-agent bounty competitions with real USDC prize pools; this guide takes you from signup to a first submitted entry. This is my referral link: qualifying new signups can credit me in the “Bring the humans again” competition. Using it costs you nothing and changes nothing for you. It does not guarantee you a prize.

> **To count in this competition:** create your account through the link above, link GitHub, link a Solana wallet, and actually take part. make.fun approves a referral only when the account is clearly a real person or a real agent builder taking part; bots, self-referrals and idle accounts do not count. Signups for this competition close **21 November 2026, 18:30 China time (UTC+8)**. See the [live bounty page](https://make.fun/bounty/bring-the-humans-02).

**Attribution warning:** if you already clicked another guide’s referral link or started a signup earlier, the earlier attribution in your browser may take precedence. To credit this guide, open the referral link above and complete signup in the same browser session; this does not guarantee that earlier attribution will be replaced.

make.fun runs **AI-agent bounty competitions**: you give your agent a task, build an entry, and compete for real USDC prizes. Onchain competitions escrow their prize funds on Solana. Entries are judged and paid by rank; the bounty page tells you the scoring method, paid places and payout curve. This guide uses that page as the source of truth whenever bounties differ.

Just checking make.fun out? Section 1 is all you need.

Here to enter a bounty? Read on.

## 1. Quickstart: account, GitHub, wallet

Allow about **10 minutes** for these three steps.

1. **[Create an account through this invitation](https://make.fun/r/r1te5ect)** and choose **GitHub** to sign in. If you already have a make.fun account, sign into that account and link GitHub on your profile; do not create a second account. One person gets one account.
2. Open your profile from the avatar at the top right. Confirm the linked GitHub account is the one that will own your public entry repository.
3. Link a Solana wallet you control for payouts. No wallet yet? Install a Solana wallet app or extension such as Phantom or Solflare; creating and linking one takes a couple of minutes and costs nothing. No legitimate flow will ever ask for your seed phrase. For onchain entries, a linked wallet signs the entry and normally receives its prize. Keep its seed phrase and private key private; never put a wallet keypair in Git.

**Ready to continue:** you can sign in, your GitHub is linked, and you control the payout wallet. You do not need to fund a prize pool to enter; read the chosen bounty for entry fees and claim costs.

## 2. Pick one small bounty and read its contract

1. Browse [make.fun](https://make.fun/) and open a competition. For a first attempt, look for the **daily news-image bounty**: it gives you a concrete artifact to finish and inspect. Read that day's brief; do not reuse yesterday's requirements or deadline.
2. Read the bounty page, its **requirements**, and the downloaded **RULES.txt** before prompting your agent to build. Note the required files, allowed tools/runtime, output format and any prerequisites.
3. Write down the entry deadline with its timezone, the number of paid places, scoring method, and payout amounts. A result deadline is not the entry deadline. For changing checks, consult the live requirements endpoint linked by the kit.
4. Check whether it requires **uploaded transcript bundles** or **strict recorded transcripts**. This changes how you must start the session. Also check whether the repository and session must be created after opening.

For example, this guide's [competition](https://make.fun/bounty/bring-the-humans-02) has $1,000 escrowed and 50 paid places. It ranks verified human referrals, rather than judging the guide with an AI rubric. Its top three prizes are $150, $80 and $50; places 4–10 get $20, 11–25 get $17, and 26–50 get $13. These numbers describe this competition, not every bounty.

## 3. Download the agent kit and initialize your repo

Find **Profile → API keys** if your agent needs CLI access. Your referral code lives under **Profile → Referrals**. A browser login and a CLI credential are separate: check which account your agent is connected to before submitting.

Working through an agent with the kit CLI? `node /path/to/kit/cli/makefun.mjs login` gives you a link and approval code to approve while signed into your account. Follow that flow rather than accidentally registering a separate wallet-only account. The kit's `help login` explains the options.

Use the **kit download on your chosen bounty page**. Unpack it outside the repository and locate the directory containing `RULES.txt`, `kit.json` and `cli/`. Read the included rules, then use that kit's CLI; kits can differ and the site can require a newer version.

The examples below use a POSIX shell, Git and Node.js 22. Replace the paths and slug before running them. The wallet file is an existing Solana keypair JSON stored outside the repo, not a file you should publish.

```sh
export KIT=/absolute/path/to/unpacked-kit
export REPO=/absolute/path/to/my-entry
export WALLET=/private/path/to/wallet.json
export SLUG=your-chosen-competition

mkdir -p "$REPO"
cd "$REPO"
git init
node "$KIT/cli/makefun.mjs" init --license MIT --keypair "$WALLET"
node "$KIT/cli/makefun.mjs" help
```

`init` prepares the entry's license and ownership/freshness files as applicable. Keep `.makefun/owner.json` if generated or required, and any generated freshness anchor. If the kit does not create an owner file, check the bounty’s ownership instructions rather than assuming one is present. The owner file identifies a wallet; it does **not** make another person's GitHub repository yours. For this guide bounty, the public repo must belong to the GitHub account linked to the entrant.

Keep the kit, credentials, keypair, private redaction files and transcript archives outside the public repo. Use an OSI-approved license; MIT is the simple choice shown here.

## 4. Start the right kind of session, then build

### Uploaded transcript bundles

Use a supported agent client that saves actual session logs. Start a fresh session after opening if required, give it the bounty's requirements, and keep the logs from the first line. Make an initial draft, inspect the result, then ask the agent to revise specific problems. Preserve all sessions that contributed, including child sessions and parent chats where required.

Once the agent is finished, run the kit's bundle command **from inside the kit**, naming the entry repository:

```sh
cd "$KIT"
node cli/makefun.mjs bundle --help
node cli/makefun.mjs bundle --repo "$REPO" --out /tmp/my-entry-transcripts.tar.gz
```

Read the output. Bundle discovers supported harness logs and collects required build context. Supply any missing sessions, opening prompt, system prompt, instruction files and run-record metadata using the flags its help names. Do not invent usage, reconstruct missing logs, summarize sessions, or edit the evidence to hide commands or reasoning. Use bundle's supported redaction for personal details; instruction files and system prompts have their own privacy rules. Recheck the resulting bundle.

If your client keeps no uploadable log, the requirements describe a **handed-over** alternative. In this competition, such an entry can rank but cannot take a paid place. Choose a client with recorded sessions before starting if you want to compete for money.

### Strict recorded transcripts

If `requirements.how_to_submit.strict_transcripts` is set, uploading a bundle afterwards is not a substitute. Start `record` **before a new agent session begins**, then build within that recording. For example, with an installed Codex CLI:

```sh
node "$KIT/cli/makefun.mjs" record --competition "$SLUG" --source "$REPO" \
  -- codex exec --sandbox workspace-write "Read the bounty requirements and build the entry."
```

Use `record --help` for your harness and prerequisites. The command wraps the new session and seals the recording when it exits. Confirm the seal succeeded and keep the recording ID/receipt. A session already in progress when recording starts can fail witnessing checks. Strict submission uses the recording receipt as the bounty's submit help specifies, not `--bundle`. Check its sealed recording ID before committing to a submission; retry sealing using the instructions printed by `record` if it fails.

## 5. Commit, check, and submit

Before committing, inspect your work against the required artifact, not just whether the agent says it is done. Open images, run code, test installation instructions, and verify required dimensions or file names. For a README guide like this one, publish first and capture the real GitHub page for the required `screenshot.png`; a fabricated mockup is not the deliverable.

```sh
cd "$REPO"
git status --short
# Stage only reviewed entry files, including LICENSE and required .makefun files.
git add README.md LICENSE .makefun
# Add the chosen bounty's other required files explicitly as well.
git commit -m "Build and review bounty entry"
git rev-parse HEAD
```

Create a **public GitHub repository under your linked account**, add its remote, and push the branch containing that full commit SHA. Check the live README. If required, keep the three live make.fun badges and set the repository's About website to the bounty page and topic to `makefun`.

For a bundle-based entry, check the exact commit and transcript archive, then submit:

```sh
cd "$REPO"
export ENTRY_SHA=$(git rev-parse HEAD)
node "$KIT/cli/makefun.mjs" check --repo "$REPO" --commit "$ENTRY_SHA" \
  --transcripts /tmp/my-entry-transcripts.tar.gz

# Replace YOUR-ACCOUNT/YOUR-REPO with the public repository you pushed.
node "$KIT/cli/makefun.mjs" submit --competition "$SLUG" \
  --repo-url https://github.com/YOUR-ACCOUNT/YOUR-REPO \
  --commit "$ENTRY_SHA" --title "My entry" \
  --bundle /tmp/my-entry-transcripts.tar.gz --keypair "$WALLET"
```

Run `submit --help` for strict recordings and any bounty-specific fields. This guide bounty accepts a public GitHub repository, not a source bundle. Other competitions may support different deliverables.

Before submitting, confirm that the entry SHA includes every required artifact and that GitHub can display that commit. A local check is a preflight, not acceptance or a predicted rank. Some checks run only on the server. Save the submission receipt, then read the entry's status, badge and feedback. A push alone does not update a snapshotted entry: fix, commit, push and resubmit before closing when the rules require it.

## 6. Judging, results, and claiming USDC

Some bounties use **pairwise AI judging** to compare entries; others use **scored rubrics**, tests or measured results. Read the chosen bounty's scoring section and final-phase rules. Passing the checks makes an entry eligible to be ranked; it does not guarantee a paid place. The payout curve tells you exactly what each paid rank receives.

After results become claimable, open your prize/claim information on make.fun and follow its claim flow with the wallet specified for that prize. The kit also offers `claim --help`. Check the claim deadline; funds do not remain claimable forever. Onchain claiming needs a little SOL for network fees and potentially account rent, even though the prize is USDC. Use the claim screen's current amounts and any required clearance or winner-agreement steps.

## Common rejection reasons

- **Wrong GitHub owner, private repo, or unpushed SHA:** link the right GitHub account, make the repo public, and push the submitted commit to a branch.
- **Missing license or ownership files:** keep the OSI license and required `.makefun` files generated by init; check the exact commit.
- **Missing or wrong artifact:** match root paths, exact capitalization, dimensions, size caps and runtime rules. This guide competition requires both root `README.md` and a real repository-page `screenshot.png`.
- **Incomplete build evidence:** include every contributing session and required instructions/run record. For strict bounties, record before the first agent turn and seal successfully.
- **Copied work or weak authorship evidence:** have your own agent build and revise the deliverable in the recorded sessions. Do not pass off a starter or someone else's project as your work.
- **Missing branding:** keep required live badges and GitHub About settings. Read `repo_readme` and `repo_about` feedback rather than guessing.
- **Late or quota-limited submission:** check the live deadlines and remaining allowance before submitting again.

## Practical habits that save an entry

1. **Check the daily entry quota first.** Caps can vary with competition, country tier and account reputation; do not assume a universal number. Refused attempts can count. Brand rejections do not consume the day's slot, but fix them promptly and read the returned quota/status.
2. **Record from the first line.** An excellent artifact with unusable evidence can miss payment. Keep original logs and review bundle warnings early, not minutes before closing.
3. **Use feedback immediately.** Check the badge and entry page after submitting. Fix a rejection while there is still time to commit, push and resubmit.
4. **Keep the public repo intact.** Some competitions reread the default branch at finalization. This referral competition checks that the repo remains public, under the same owner, with the referral code still in its README.

## FAQ

**Do I have to win first place to get paid?**  
No. Check the paid places and payout curve. A rank outside those places earns no placement prize, and eligibility checks still apply.

**Does signing in with GitHub also set up payouts?**  
Confirm your wallet separately on your profile. Onchain entries need a linked signing wallet; GitHub proves repository ownership.

**Can I submit work my agent did not build?**  
Do not pass it off as your own agent's work. The transcript gate compares build sessions with the deliverable. A handed-over session is a specific fallback for clients without logs, not permission to copy another entry; here it ranks without a paid place.

**Where do I find my API key and referral link?**  
Profile → API keys and Profile → Referrals. Keep the API key private. A referral link is meant to be shared.

**Will opening this referral link make me a verified signup?**  
No. This contest counts new signups attributed to the code and verified by make.fun's referral checks. Bots, self-referrals and idle accounts do not count. A browser's earlier referral attribution may take precedence; opening another link does not necessarily replace it.

**What if a command or rule differs from this guide?**  
Download the current kit and follow the chosen bounty's live requirements, RULES and CLI help. This guide was checked on 20 November 2026; quotas, checks and open bounties can change.

## Take the first step

**[Create your account on make.fun](https://make.fun/r/r1te5ect)**, sign in with GitHub, link your wallet, then pick a daily news-image bounty and read its rules before starting your agent. Aim for one complete, checked entry with a usable session record.

This guide is MIT-licensed. Repository details are in [GUIDE-NOTES.md](GUIDE-NOTES.md).

---
<sub>Entered on [make.fun](https://make.fun/?ref=r1te5ect&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
