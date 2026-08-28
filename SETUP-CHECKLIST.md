# What you do by hand

Ordered. Nothing here can be done for you.

## 1. Create the profile repo

On GitHub: **New repository** → name it exactly `oneill162` → **Public** →
create. GitHub will show a "special repository" notice; that is the one whose
README renders on your profile. Then, from this folder:

    git remote add origin https://github.com/oneill162/oneill162.git
    git push -u origin main

## 2. Banner SVGs

Drop `dark.svg` and `light.svg` into `assets/`. The README already points at
`main/assets/`. Nothing renders until these exist.

## 3. GitHub token for the stats cards

Settings → Developer settings → Personal access tokens → **Tokens (classic)** →
Generate new token (classic) → scope **`repo`** → expiration **No expiration**.

Copy it immediately — GitHub shows it once. Never paste it into a README, an
issue, a commit, or a chat window. It grants full access to your repositories.

## 4. Self-host the stats cards

The public github-readme-stats instance is shared by thousands of profiles and
returns "API rate limit exceeded" for most of the day. Self-hosting is the fix.

1. Fork `anuraghazra/github-readme-stats`
2. Go to vercel.com, sign up with GitHub, choose the **Hobby** (free) plan
3. **Add New Project** → import your fork
4. Add an environment variable `PAT_1` = the token from step 3
5. **Deploy**, then copy your instance URL

Then replace both `YOUR-INSTANCE.vercel.app` placeholders in `README.md`.

The cards use `hide_rank=true`. The rank is stars-weighted, so a newer or
teaching-focused account gets a low letter grade that reflects repo popularity
rather than activity. Hiding it is more accurate, not more flattering.

## 5. Enable Actions write permission

Repo **Settings** → **Actions** → **General** → Workflow permissions →
**Read and write permissions** → Save.

This is the *repository's* settings page, not your account settings. The snake
workflow cannot push its output branch without it.

## 6. Run the snake workflow

Actions tab → **Generate contribution snake** → **Run workflow**.

Wait for a green check. The workflow creates the `output` branch on its first
successful run — before that, the snake `<picture>` block in the README points at
a branch that does not exist and will show broken images. That is expected.
After it goes green, hard-refresh your profile.

## 7. Missing inputs

The banner spec drops `Grid.LinkedIn` and `Grid.Facebook` because no URLs were
supplied. If you want them, send the URLs.

If you add a LinkedIn badge later: its logo only renders on brand blue
`#0A66C2`. On any custom colour the glyph silently vanishes and you get a
text-only badge. Either use brand blue or embed the glyph as a base64 data-URI —
the Email badge in this README already uses that technique, because Simple Icons
dropped the `microsoftoutlook` logo and `logo=microsoftoutlook` now returns a
badge with no glyph at all.
