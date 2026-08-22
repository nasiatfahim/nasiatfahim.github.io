# Publishing to nasiatfahim.github.io

Every file in this folder already points at `nasiatfahim.github.io` and `github.com/nasiatfahim`.
Follow the steps in order. Total time: about 20 minutes.

---

## Step 1 — Confirm the rename landed (2 minutes)

You've renamed your account to `nasiatfahim`, so `nasiatfahim.github.io` is yours to claim.
Two quick checks before you build on it:

1. Open `https://github.com/nasiatfahim` — your profile should load with all your
   repositories intact.
2. If you have any local clones, their `git remote` URLs still point at the old name.
   GitHub redirects them, so they keep working, but it's cleaner to update:

   ```bash
   cd path/to/some-repo
   git remote set-url origin https://github.com/nasiatfahim/<repo>.git
   ```

**Loose ends the rename created.** GitHub redirects the old URLs, but only until someone
else claims `Md-Nasiat-Hasan-Fahim`. That's unlikely for a string that specific, though it
means these need updating on your schedule:

| Where | Says now | Should say |
|---|---|---|
| **Your CV PDF** | `Md-Nasiat-Hasan-Fahim` | `nasiatfahim` — regenerate before your next application |
| LinkedIn contact info | old handle | new handle |
| Any paper, dataset or submission with a GitHub URL | old handle | new handle |

The CV is the one that matters. It's the document that will be forwarded around a hiring
committee without you in the room, and a handle that redirects today may not in five years.

---

## Step 2 — Create the repository

New repository → and get these exactly right:

| Field | Value | Why |
|---|---|---|
| **Repository name** | `nasiatfahim.github.io` | Must match your username exactly, all lowercase. This *is* what makes GitHub serve it at the root domain. |
| **Visibility** | **Public** | Pages on a private repo needs a paid plan. |
| Add a README | **unticked** | |
| Add .gitignore | **unticked** | An empty repo makes both upload paths below cleaner. |
| Choose a license | **unticked** | |

Click **Create repository**. You'll land on a "quick setup" page — keep it open.

---

## Step 3 — Upload the files

Pick **A** if you want it live in five minutes with no tooling. Pick **B** if you want a
sane way to update it for the next four years. You are a CS graduate; pick B.

### Path A — browser upload, no git

1. Unzip the `website` folder and open it. You should see `index.html` sitting directly
   inside.
2. On the empty repo page, click **uploading an existing file**.
3. Open the `website` folder, select **everything inside it** (`Ctrl`/`Cmd` + `A`), and drag
   the selection into the browser window.

   > **This is the step people get wrong.** Drag the *contents*, not the `website` folder
   > itself. If the folder goes in, your files land at `nasiatfahim.github.io/website/index.html`
   > and the site returns 404. `index.html` must sit at the top level of the repository.

4. Wait for the upload to finish — `assets/` contains your CV PDF and portrait, so give it
   a few seconds.
5. Commit message: `Publish site`. Click **Commit changes**.
6. The web uploader silently skips files beginning with a dot, so `.nojekyll` won't make it.
   The site works fine without it. If you want it anyway: **Add file → Create new file**,
   name it `.nojekyll`, leave the body empty, commit.

### Path B — git command line

```bash
cd path/to/website          # the folder containing index.html
git init
git add -A
git commit -m "Publish site"
git branch -M main
git remote add origin https://github.com/nasiatfahim/nasiatfahim.github.io.git
git push -u origin main
```

If it prompts for a password, note that GitHub removed password authentication years ago.
Either:

- **Token:** Settings → Developer settings → Personal access tokens → Tokens (classic) →
  Generate new token → tick the `repo` scope → paste the token where it asks for a password.
- **Or GitHub CLI:** install `gh`, run `gh auth login`, and it handles this for you.

---

## Step 4 — Switch Pages on

Repository → **Settings** → **Pages** (left sidebar).

- **Source:** Deploy from a branch
- **Branch:** `main` · folder `/ (root)` → **Save**

For a `username.github.io` repo this is often enabled automatically — if it already says
`main` / `root`, you're done.

The first build takes **1 to 10 minutes**. Watch the **Actions** tab for a green tick, or
just refresh Settings → Pages until the banner reads *"Your site is live at
https://nasiatfahim.github.io/"*.

---

## Step 5 — Verify before you send the link anywhere

Work through all of these. A broken link on a site you put on a job application is worse
than no site.

- [ ] `https://nasiatfahim.github.io/` loads, and the seismogram draws itself across the hero
- [ ] Body text renders in a serif (Spectral). **If it looks like Times New Roman**, the
      stylesheet or Google Fonts didn't load — see troubleshooting
- [ ] Every nav item opens: Research, Publications, Teaching, Projects, CV
- [ ] **PDF ↓** downloads your CV, and the CV that downloads is the current one
- [ ] The portrait is your photograph, not the grey placeholder
- [ ] Scholar and ORCID links go somewhere real — **or are deleted**. A dead link is worse
      than a missing one
- [ ] Open it on your phone
- [ ] Open it on a device you haven't touched (borrow a friend's laptop). Your browser
      cache hides problems from you specifically

---

## Step 6 — Updating it later

**With git:** edit the file, then

```bash
git add -A && git commit -m "Add ICCIT dataset link" && git push
```

Live in about a minute.

**In the browser:** open the file on GitHub → pencil icon → edit → **Commit changes**.

To add a publication, edit `_tools/_pages_a.py`, then run
`cd _tools && python3 _pages_a.py && python3 _pages_b.py` and push the regenerated HTML.
`index.html` is hand-written — edit it directly.

---

## Troubleshooting

| What you see | Cause | Fix |
|---|---|---|
| 404 on the whole site | Files are nested in a subfolder | The repo root must contain `index.html`. Move files up a level. |
| Page loads but has no styling | `assets/site.css` missing or misnamed | GitHub Pages is **case-sensitive**: `Site.css` ≠ `site.css` |
| Text is Times New Roman | Google Fonts didn't load | Usually an ad blocker or a network that blocks `fonts.googleapis.com`. Test on mobile data |
| Still showing the old version | Browser cache | Hard refresh: `Ctrl`+`Shift`+`R` (Win) / `Cmd`+`Shift`+`R` (Mac), or open a private window |
| Portrait shows the grey placeholder | Filename mismatch | Must be exactly `assets/portrait.jpg`. `.jpeg` is not `.jpg` |
| CV link 404s | Filename mismatch | Must be exactly `assets/Md-Nasiat-Hasan-Fahim-CV.pdf` |
| Pushed, but nothing changed | Build still running, or push went to the wrong branch | Check the Actions tab; confirm you pushed to `main` |
| Pages tab has no "Source" option | Repo is private | Make it public: Settings → General → Danger Zone → Change visibility |

---

## Later: a custom domain

`nasiat.com` is already taken by a news site, so aim at something like `nasiatfahim.com`
(roughly $12/year). Once you own one:

1. Create a file named `CNAME` at the repo root containing only `nasiatfahim.com`
2. At your registrar, add four `A` records pointing to `185.199.108.153`,
   `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
3. Settings → Pages → Custom domain → enter it → tick **Enforce HTTPS** once the
   certificate is issued

Worth doing before your main application round. A custom domain survives any future
username change, and `nasiatfahim.github.io` on a CV quietly signals "student project" in a way
`nasiatfahim.com` does not.
