# Alexis (Li) Lu — Personal Academic Website

Static site, three pages, no build step. Editorial / print-inspired design:
warm paper ground, serif display type (Newsreader), asymmetric left-aligned
grids, hairline rules, one restrained accent.

```
site/
├── index.html      Home — opening spread, portrait, dossier rows, honours, media exposure
├── research.html   Research — lede, works in progress, research experience, toolkit, contact
├── teaching.html   Teaching — photo band, lede, syllabus-style course index, photo band
├── phd.html        PhD at Ivey — supervisors, doctoral coursework
├── paper-*.html    One title page per working paper: title, authors, abstract, keywords
├── style.css       All styling; design tokens at the top
├── profile.jpg     Portrait (circle crop)
├── home-hero.jpg   Home band (from Class.HEIC, pre-blurred Gaussian r=10)
├── China.png       Research band, shown whole
├── Ivey.png        PhD at Ivey band
├── teaching-hero.jpg / teaching-exam.jpg   Teaching bands
└── Alexis_Lu_CV.pdf   Published CV (stable filename, so the link never changes)
```

## Design tokens (`style.css`, `:root`)

| Token | Now | What it controls |
|---|---|---|
| `--paper` / `--paper-2` | `#FBF8F3` / `#F2EDE4` | page ground, footer + tint blocks |
| `--ink` / `--ink-2` / `--muted` | `#14130F` / `#45413A` / `#857E71` | headings, body, labels |
| `--accent` | `#2F5D53` (deep pine) | links, course codes, section rules |
| `--rule` / `--rule-soft` | `#E3DCCF` / `#EDE7DC` | hairlines |
| `--font-display` | Newsreader | headings and body prose |
| `--font-ui` | Inter | nav, labels, course codes, buttons |
| `--shell` / `--measure` | `1080px` / `68ch` | page width, prose measure |

## To personalize

- **Portrait** — replace `profile.jpg`. It is a circle; framing is `object-position` on
  `.portrait img` and size is `max-width` on `.portrait`. Caption lives in `index.html`
  (`<figcaption>`).
- **Page backgrounds** — every page head is a `.band` with a `.band__media--*` layer defined in
  `style.css`: `--home` (cover), `--map` (Research, `background-size: contain` so the whole map
  including the Google attribution stays visible), `--ivey` (cover), `--hero` / `--exam`
  (Teaching). Crop is `background-position`; the darkening wash is the `::after` on each band.
- **Home blur** — `home-hero.jpg` is blurred in the file itself, not in CSS. CSS `filter: blur()`
  on a negative-z-index background layer washes the image out to near black in this layout, so
  re-blur from the sharp copy instead:
  `python3 -c "from PIL import Image,ImageFilter; Image.open('../home-hero-original.jpg').filter(ImageFilter.GaussianBlur(10)).save('home-hero.jpg',quality=86)"`
  then bump the `?v=` on the image URL in `style.css`.
- **Cache busting** — image URLs and the `style.css` link carry `?v=` stamps. Bump them whenever
  you replace a file with the same name, otherwise browsers keep showing the old one.
- **Teaching photos** — the Teaching page has two full-bleed photo bands. Drop
  `teaching-hero.jpg` (top band, behind the heading) and `teaching-exam.jpg` (the short band
  between Courses and Honours) into this folder. `teaching-exam.jpg` is blurred by CSS, so save
  the original file unmodified. Both are referenced from `.band__media--hero` and
  `.band__media--exam` in `style.css`; the crop is `background-position` and the blur strength
  is the `filter` on `--exam`. If a file is missing the band falls back to a dark gradient.
- **Papers** — the site shows a title page per paper, not the full draft. Each
  `paper-*.html` carries the title, authors, abstract, keywords and a schema.org
  `ScholarlyArticle` block; `research.html` links to them. To add a paper, copy an existing
  `paper-*.html`, edit the content, link it from `research.html` and add it to `sitemap.xml`.
- **Contact rule** — the email address is shown as plain text everywhere, never as a mailto
  link; LinkedIn is always the live link. The CV is linked from the Home page only.
- **Accent** — one variable, `--accent`. A warm alternative that suits the paper ground
  is `#8A4B2F` (clay); a cooler one is `#28415C` (ink blue).
- **CV** — published as `Alexis_Lu_CV.pdf`, linked from the Home hero button and the Contact
  row. Keep that filename when you update it so the link never breaks. The current file is the
  September 2026 CV; it does list a phone number, which Alexis decided was fine.

## Preview locally

```
python3 -m http.server 4321 --directory site
```

## Published

This folder is the git repo behind <https://alexli-lu.github.io> (GitHub Pages, `main` branch,
root). To update: edit, `git add -A`, `git commit`, `git push`. Pages redeploys in about a
minute.

SEO: `robots.txt`, `sitemap.xml`, canonical links, Open Graph tags and a schema.org `Person`
block on the Home page. `.nojekyll` stops GitHub from running Jekyll over the files.
