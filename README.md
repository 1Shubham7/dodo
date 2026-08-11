# dodo 🦤

**dodo** is a job-hunting agent that lives in your repo. Open this repository in
**Claude Code**, **Codex**, or **Cursor** and the agent is already there — no install,
no server, no accounts. It builds your resumes in LaTeX, tailors them to job
descriptions, answers application forms in your voice, and gets smarter about you with
every application you feed it.

## What it does

1. **Builds your resumes.** Give dodo an old resume (or just tell it about yourself),
   tell it which roles you're targeting, and it creates one tailored LaTeX resume per
   role in `resume/` — same facts, different emphasis for each role.

2. **Learns from your applications.** Share the applications you've filled before, as
   many as you can. dodo stores each one in its own folder (e.g. your CERN application
   goes to `applications/cern/`) and mines them for facts and stories about you.

3. **Answers new application forms.** Ask dodo to answer a question or a whole form —
   it drafts answers from everything it knows about you, reusing your own past answers
   where they fit. Once you confirm the answers look good, it stores them for next time.

4. **Adapts your resume to a JD.** Paste a job description and dodo recommends which
   base resume fits best, then (if you want) adapts it to the JD — mirroring the JD's
   keywords, leading with the most relevant experience, cutting the rest — and saves it
   in that company's folder.

5. **Never lies.** dodo rephrases, reorders, and emphasizes what's true about you. It
   does not invent experience, numbers, or skills.

## Getting started

```bash
git clone <this-repo>
cd dodo
claude        # or: codex, or open the folder in Cursor
```

Then just say:

> hey, help me with my job hunt

dodo will ask whether you have a resume or want one created, and take it from there.

Everything dodo learns about you lives in this repo as plain files, so you own it, you
can read it, and you can version it with git.

> **Note:** if you push this repo anywhere, keep it **private** — it will contain your
> resume, contact details, and application answers.

## Repository layout

```
AGENTS.md             # The agent definition (read by Codex & Cursor)
CLAUDE.md             # Entry point for Claude Code (imports AGENTS.md)
.cursor/rules/        # Entry point for older Cursor versions
templates/resume.tex  # Standard ATS-friendly LaTeX resume template
profile/              # Master profile — everything dodo knows about you
resume/               # One LaTeX resume per target role
applications/         # One folder per company
  <company>/
    jd.md             # The job description
    application.md    # Questions + your confirmed answers
    resume.tex        # Resume adapted for this specific JD
```

## Compiling resumes

dodo writes resumes as LaTeX. If you have a TeX toolchain it will compile PDFs for you:

```bash
latexmk -pdf resume/backend-engineer.tex    # or: pdflatex, run twice
```

No TeX installed? Drop any `.tex` file into [Overleaf](https://overleaf.com) and it
compiles there.

## How it works

There's no code here — dodo is a set of instructions in [`AGENTS.md`](AGENTS.md) that
coding agents pick up automatically:

- **Codex** and **Cursor** read `AGENTS.md` natively.
- **Claude Code** reads `CLAUDE.md`, which imports `AGENTS.md`.
- Older Cursor versions pick it up via `.cursor/rules/dodo.mdc`.

Want to change how dodo behaves? Edit `AGENTS.md` — it's just markdown.

## License

See [LICENSE](LICENSE).
