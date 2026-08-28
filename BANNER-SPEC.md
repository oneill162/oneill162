# Banner spec — Phase 1 source of truth

Canvas 1180×610, terminal window titled `profile.sh --live`.
Left ~38% = portrait frame labelled `VISUAL.MAP`. Right = `SYSTEM.INFO` readout.

## Palette

| Role | Dark | Light |
|---|---|---|
| Portrait dots | `#A78BFA` | `#7C3AED` |
| UI chrome | `#22D3EE` | `#0891B2` |
| Accent | `#10B981` | `#10B981` |
| Background | `#0A101F` | `#FFFFFF` |

Portrait hue (violet) is deliberately distinct from chrome hue (cyan) so the face
does not blend into its own frame.

## Morph logos

Android → Node.js → Linux (Tux). Trace from reference PNGs; do not hand-draw.

## SYSTEM.INFO rows

Font-size 14, header 13, LIVE badge 12, pill 14, row spacing 23px.
Every row locked with `textLength` + `lengthAdjust="spacingAndGlyphs"`.
Dotted leaders computed from label/value length — never hand-edited.

| Row | Value |
|---|---|
| `Subject` | Eduardo O. De Jesus Arroyo |
| `Role` | Computer Systems Professor | Full-Stack Developer |
| `Origin` | Canovanas, Puerto Rico |
| `Edu.Grad` | M.S. Cybersecurity - Ana G. Mendez Univ. (2022) |
| `Edu.Undergrad` | B.B.A. Computerized Information Systems - AGM (2015) |
| `Edu.Cert` | K-3 Education Certification - Caribbean Univ. PR |
| `Status` | Teaching systems, programming & IT - building in the open |
| `ToolChain` | VS Code, Git, Android Studio, MS 365, AI-assisted dev |
| `Core.Lang` | JavaScript, Java, Kotlin, Python, PHP, C#, C++, SQL |
| `Core.Frontend` | HTML5, CSS3 |
| `Core.Backend` | Node.js, Express, Laravel, ASP.NET, Spring Boot |
| `Core.Database` | MySQL, Supabase |
| `Core.Mobile` | Android, Kotlin, MVVM, Retrofit, Coroutines, REST |
| `Core.Infra` | Ubuntu, Linux, Windows Server, TCP/IP, DNS/DHCP |
| `Core.Security` | Systems, Network & Security Fundamentals |
| `Grid.Mail` | e.oneill162@outlook.com |
| `Grid.Portfolio` | Coming Soon |
| `Grid.Instagram` | @zafiro.tech |
| `Grid.GitHub` | oneill162 |

Handle pill: `@oneill162` on the accent green.

19 rows x 23px = 437px of readout. Values are transcribed verbatim from the
approved render (`6-banner-both-modes.png`), which is the reference any rebuild
must match.

Education is split across three rows rather than the master prompt's single
`Education` row, and `Grid.LinkedIn` / `Grid.Facebook` are dropped for lack of
URLs. Accented characters are stripped (Canovanas, Mendez) to stay inside the
monospace face used by the panel.

## Portrait pipeline

- Head + shoulders crop, not a tight face crop
- 300×340 grid → 1-bit Floyd–Steinberg dither, serpentine order
- Contrast 1.3× only, `autocontrast(cutoff=1)`, `UnsharpMask(radius=3, percent=140)`
- Dots as `<path>` runs with `shape-rendering="crispEdges"` — never font glyphs
- Dark: segment background out (colour-distance threshold, binary closing, fill
  holes, keep largest component), hard-clear error-diffusion bleed at mask edge
- Light: keep background; dots draw the dark parts of the photo
- Single hue, all tone from dot density. No grid lines, scanlines, or glitch bars

## Animation

**Intro** ~3.2s once: ~60 interleaved random groups fade in over ~2s, each group
scattered across the whole portrait. Not a wipe, not grouped by region. Evenness
metric ~0.05 good / ~0.7 patchy. Requires a duplicate portrait layer (~180KB).

**Loop** ~14.2s: portrait 3.0s, each logo 2.0s, 1.3s transitions, explicit uneven
`keyTimes`.

Two independent layers:
1. **Portrait** — full density (~17k dots) in ~94 drift bands; each band
   translates ~42% toward the first logo's centroid while fading, then returns.
2. **Travellers** — ~900 dots morphing between logos, matched by optimal
   transport. Opacity keyframes `0;0;0;1;1;…;0` so they stay hidden during the
   portrait phase.

Add per-dot noise (sigma ~4) **before** grouping into drift bands. Drift is a
linear function of position, so quantizing without noise mathematically recreates
a square grid and the dissolve looks blocky. Straight-boundary metric: ~0.01
organic, ~0.17 means a grid was built.

## Verified render counts (from the approved session)

- dark: 32,783 dots
- light: 31,445 dots
- drift bands: 94
