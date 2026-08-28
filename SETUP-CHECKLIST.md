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

## 3. Stats cards - removed

The GitHub-stats and top-languages cards were dropped from the README. They
required self-hosting `github-readme-stats` on Vercel with a personal access
token, and the public instance was returning `DEPLOYMENT_PAUSED`.

No token and no Vercel account are needed for anything that remains.

To add them back later: fork `anuraghazra/github-readme-stats`, import it to
Vercel on the free Hobby plan, set an environment variable `PAT_1` to a classic
token with `repo` scope, deploy, then add two `<img>` tags pointing at your
instance with `hide_rank=true` and the palette in BANNER-SPEC.md.

The streak card that remains is not affected - it needs no token.

## 4. Enable Actions write permission

Repo **Settings** → **Actions** → **General** → Workflow permissions →
**Read and write permissions** → Save.

This is the *repository's* settings page, not your account settings. The snake
workflow cannot push its output branch without it.

## 5. Run the snake workflow

Actions tab → **Generate contribution snake** → **Run workflow**.

Wait for a green check. The workflow creates the `output` branch on its first
successful run — before that, the snake `<picture>` block in the README points at
a branch that does not exist and will show broken images. That is expected.
After it goes green, hard-refresh your profile.

## 6. Missing inputs

The banner spec drops `Grid.LinkedIn` and `Grid.Facebook` because no URLs were
supplied. If you want them, send the URLs.

If you add a LinkedIn badge later: its logo only renders on brand blue
`#0A66C2`. On any custom colour the glyph silently vanishes and you get a
text-only badge. Either use brand blue or embed the glyph as a base64 data-URI —
the Email badge in this README already uses that technique, because Simple Icons
dropped the `microsoftoutlook` logo and `logo=microsoftoutlook` now returns a
badge with no glyph at all.
