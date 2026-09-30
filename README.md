# dodo 🦤

**dodo** is a job-hunting agent that lives in your repo. Open this repository in
**Claude Code**, **Codex**, or **Cursor** and the agent is already there - no install,
no server, no accounts. It builds your resumes in LaTeX, tailors them to job
descriptions, answers application forms in your voice, and gets smarter about you with
every application you feed it.

## What it does

1. **Builds your resumes.** Give dodo an old resume (or just tell it about yourself),
   tell it which roles you're targeting, and it creates one tailored LaTeX resume per
   role in `resume/` - same facts, different emphasis for each role.

2. **Learns from your applications.** Share the applications you've filled before, as
   many as you can. dodo stores each one in its own folder (e.g. your CERN application
   goes to `applications/cern/`) and mines them for facts and stories about you.

3. **Answers new application forms.** Ask dodo to answer a question or a whole form -
   it drafts answers from everything it knows about you, reusing your own past answers
   where they fit. Once you confirm the answers look good, it stores them for next time.

4. **Adapts your resume to a JD.** Paste a job description and dodo recommends which
   base resume fits best, then (if you want) adapts it to the JD - mirroring the JD's
   keywords, leading with the most relevant experience, cutting the rest - and saves it
   in that company's folder.

5. **Fills application forms in your browser.** Give dodo the form's URL and it fills
   the fields for you, page by page. You edit whatever you don't like directly in the
   browser, then dodo captures the final text from the page and stores it for future
   applications. It clicks Next, but never Submit: that click is always yours.

6. **Never lies.** dodo rephrases, reorders, and emphasizes what's true about you. It
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

> **Privacy:** your data is never pushed to GitHub. The `profile/`, `resume/`, and
> `applications/` folders are gitignored (only the empty folder structure is tracked),
> so everything dodo learns about you - your resume, contact details, and application
> answers - stays on your machine. On top of that, dodo will never commit or push your
> private files on its own; it always asks first. If you do want to version your data,
> remove those entries from `.gitignore` and keep your fork **private**.

## How to use the agent

dodo works with whatever you give it. Here is what to provide, where, and how.

### 1. Your existing resume (if you have one)

Give dodo your current or old resume in any of these ways:

- **Drop the file into the repo** (root or anywhere, any format: PDF, `.tex`, `.docx`,
  `.md`, `.txt`) and say: "my resume is in resume.pdf, use it"
- **Paste the text** straight into the chat
- If it is on Overleaf, download the `.tex` or PDF first and drop it in

dodo reads it, extracts everything into `profile/me.md` (its master knowledge base
about you), and works from there. You can delete the original file afterwards if you
want; the extracted profile is what dodo uses.

### 2. No resume yet? Tell dodo about yourself

Say "create a resume for me" and dodo will interview you. Have this ready:

- **Work history**: companies, titles, dates, what you built and achieved. Numbers
  help a lot (users, latency, revenue, team size).
- **Education**: degree, university, years.
- **Skills**: languages, frameworks, databases, cloud, tools.
- **Projects**: what they do, tech used, links, stars/users if any.
- **Links**: GitHub, LinkedIn, portfolio, email, phone, city.

You do not need it all polished; brain-dump in the chat and dodo will structure it.

### 3. Target roles

Tell dodo which roles you are hunting for, for example:

> I want to target backend engineer and platform engineer roles

It creates one LaTeX resume per role in `resume/` (e.g. `resume/backend-engineer.tex`),
each emphasizing the parts of your background that matter for that role.

### 4. Past applications (the more, the better)

Share applications you have already submitted anywhere: the form questions plus your
answers, cover letters, "why do you want to work here" essays. Paste them in the chat
or drop the files in the repo and point dodo at them:

> here's my old CERN application, learn from it

Each one is stored in its own folder (`applications/cern/application.md`) and mined
for facts and stories about you. This is how dodo learns to answer in your voice.

### 5. New applications

When you are applying somewhere, give dodo two things:

1. **The job description.** Always. Paste it or drop it as a file. dodo saves it to
   `applications/<company>/jd.md`.
2. **The form questions** (if you want answers drafted). dodo drafts answers from your
   profile and past applications, you review them, and once you say they look good it
   stores them in `applications/<company>/application.md` for future reuse.

For the resume, dodo will recommend which base resume from `resume/` fits the JD best.
Say "adapt it" and it tailors the resume to that JD and saves it as
`applications/<company>/resume.tex`.

### 6. Letting dodo fill the form in your browser

dodo can fill the application form itself instead of handing you text to paste. It
needs a browser tool it can drive and you can see:

- **Claude Code**: install the [Claude in Chrome](https://claude.com/chrome) extension
  and start with `claude --chrome`. dodo works in your own Chrome, with your logins.
- **Codex / Cursor**: add the [Playwright MCP](https://github.com/microsoft/playwright-mcp)
  server (headed mode, persistent profile).

Then:

1. Give dodo the form's URL: "fill this application for me: https://..."
2. dodo reads the fields, drafts answers, and fills the page. Logins, captchas, and
   sensitive questions (salary, visa status, demographics) are left to you unless you
   have answered them before.
3. Edit anything you don't like **directly in the browser**.
4. Tell dodo the page is good. It captures the final text from the page and clicks
   Next. Repeat for every page.
5. On the last page, **ask dodo to capture before you click Submit**. After submitting,
   the form is gone and cannot be read back.
6. Confirm the captured answers and dodo stores them in
   `applications/<company>/application.md`. It also compares its drafts with your
   edits, so the next form needs fewer of them.

dodo never clicks Submit. You do.

### Example session

```text
you:  help me with my job hunt
dodo: do you have an existing resume, or should I create one for you first?
you:  here's my old one (drops old-resume.pdf in the repo)
dodo: (builds profile/me.md) what roles do you want to target?
you:  backend engineer and SRE
dodo: (creates resume/backend-engineer.tex and resume/sre.tex)
you:  I'm applying to Stripe, here's the JD: ...
dodo: your backend resume fits best. want me to adapt it to this JD?
you:  yes, and answer these 3 form questions too: ...
dodo: (writes applications/stripe/jd.md, resume.tex, drafts answers)
you:  answers look good
dodo: (stores them in applications/stripe/application.md)
```

## Repository layout

```
AGENTS.md             # The agent definition (read by Codex & Cursor)
CLAUDE.md             # Entry point for Claude Code (imports AGENTS.md)
.cursor/rules/        # Entry point for older Cursor versions
templates/resume.tex  # Standard ATS-friendly LaTeX resume template
profile/              # Master profile - everything dodo knows about you
resume/               # One LaTeX resume per target role
applications/         # One folder per company
  <company>/
    jd.md             # The job description
    application.md    # Questions + your confirmed answers
    draft.md          # Working draft while a form is being filled in the browser
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

There's no code here - dodo is a set of instructions in [`AGENTS.md`](AGENTS.md) that
coding agents pick up automatically:

- **Codex** and **Cursor** read `AGENTS.md` natively.
- **Claude Code** reads `CLAUDE.md`, which imports `AGENTS.md`.
- Older Cursor versions pick it up via `.cursor/rules/dodo.mdc`.

Want to change how dodo behaves? Edit `AGENTS.md` - it's just markdown.

## License

See [LICENSE](LICENSE).
