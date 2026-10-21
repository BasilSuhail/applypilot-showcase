# ⚙️ How it works

A walk through one round, from search to Discord ping.

## 1. Find 🔎

- Searches Indeed for my target roles (data, analytics, business analysis, software and graduate schemes), posted in the last 1, 3 or 7 days.
- **Regions**, which can be picked together in one round: UK, Middle East, Europe, Canada, Pakistan. Outside the UK only the core searches run.
- Before anything is scored, duplicates are dropped: the same company and title, or the same apply link.
- Every job lands in one **master SQLite database** that is never wiped, so the bot never applies to the same job twice.

## 2. Score 🧠

- Each job description is scored **1-10** against my CV.
- **Hard blockers** cap the score: citizenship or security clearance, a required licence, a required non-English language, nationals-only roles, part-time, or contract-only.
- If the scoring model and the checking model differ, borderline scores get a second opinion.
- 5 and above moves on.

## 3. Write 📄

- A **one-page CV** tailored to the ad: reordered and reworded from my real experience, with nothing invented.
- A **cover letter** for the same ad.
- An AI judge checks the CV. A PDF that runs to two pages is never uploaded.

## 4. Apply 🤖

A Claude Code agent (Haiku) gets the job, the tailored files and a strict set of rules, then drives Chrome through Playwright MCP:

1. **Eligibility check** on the job page (location, work type) before touching the form.
2. **Login, in this order:**
   - Sign in with Google.
   - Otherwise, check Gmail for an account this bot created earlier.
   - No account: create one.
   - Account exists but sign-in fails: "Forgot password", with the reset link read from Gmail.
   - It never invents passwords and never uses LinkedIn.
3. **Fill everything** from a private profile, including right to work, salary by region, referees, screening questions, equality monitoring (neutral options) and essays to the requested length.
4. **CAPTCHA, if any**: it's done last when it sits inside the form. One beep and a 30-second wait for me; if I'm not there, the job is kept and comes up first next session.
5. **Submit**, confirm the thank-you page, and print a one-line result code.

Failures are sorted:
- **Retry**: worth another go.
- **Ineligible**: can never qualify; dropped for good.
- **Manual**: saved in a folder with the CV, cover letter and link, so I can finish it myself, or with the agent in assist mode.

## 5. Watch the inbox 📬

- A LaunchAgent runs a Gmail checker every 10 minutes, even when the bot is stopped.
- Each email is matched to the job I applied for and sorted with keyword rules:

| Type | What happens |
|---|---|
| 🟢 Interview / next steps | Discord, with a phone ping |
| 🟡 Assessment / test | Discord, with a phone ping |
| ⚪ Other real reply | Discord, quietly |
| Rejections, receipts, codes, job alerts | Not posted |

- The rules are tested against 48 labelled example emails and checked on a week of real email: 751 emails, 13 alerts, all genuine.

## 6. Report 📊

- An Excel report after every round, one row per job, with the reason for anything not applied.
- `jobbot round` lists what this round found, scored, wrote and applied, while it runs.

## Safety rails 🛡️

- Runs only when I start it, and I can stop it at any moment.
- Gmail access is read-only. The agent can't send email.
- Personal data lives only on my Mac, plus an encrypted backup on my own server.
- One page maximum for every document. No invented experience, no invented passwords.
