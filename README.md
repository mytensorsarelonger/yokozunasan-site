# yokozunasan.com

static jekyll site, hosted on github pages. no theme, no build tooling beyond
what pages runs natively. one css file. scope + rationale: `../yokozunasan-site-plan.md`.

## structure

```
_config.yml          site config; goatcounter code lives here (commented until set up)
_layouts/            default.html (shell) + post.html
_drafts/             posts-in-progress — NEVER published; move to _posts/ to publish
_posts/              published posts, filenames like 2026-07-26-slug.md
assets/css/main.css  all styling (light + dark via prefers-color-scheme)
assets/images/       post images
index.html           homepage: intro paragraph + post list
about.md             about page (identity bridge; links out to keppylab)
CNAME                custom domain for github pages
```

## deploy (one-time)

1. register `yokozunasan.com` (primary) and `yok0zunasan.com` (redirect) at any registrar.
2. create a github repo (e.g. `yokozunasan/yokozunasan.github.io` or any name), push this directory.
3. repo → Settings → Pages → Deploy from branch → `main`, `/ (root)`.
4. Settings → Pages → Custom domain → `yokozunasan.com`, check "Enforce HTTPS" once the cert issues.
5. DNS at the registrar for `yokozunasan.com`:
   - four `A` records on the apex: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `CNAME` record `www` → `<github-username>.github.io`
6. `yok0zunasan.com`: use the registrar's URL-forwarding (301) → `https://yokozunasan.com`. no github setup needed.
7. analytics: create a free account at goatcounter.com, then uncomment `goatcounter:` in `_config.yml` with your code. that's the profile→site KPI sensor — do this before swapping the X bio link.

## writing

- draft in `_drafts/<slug>.md`. publish by moving to `_posts/YYYY-MM-DD-<slug>.md`.
- front matter: `layout: post`, `title`, and `math: true` only if the page needs katex (keeps every other page JS-free apart from goatcounter).
- push to `main` = deployed. pages builds jekyll server-side; no local ruby needed.
- local preview (optional): `gem install jekyll jekyll-feed && jekyll serve --drafts`.

## launch checklist (from the site plan — order matters)

1. james rewrites: index intro, about page, both drafts (all `[JAMES: ...]` markers gone).
2. move ≥2 drafts to `_posts/`. never announce an empty site.
3. goatcounter live.
4. swap X profile website field: keppylab.com → yokozunasan.com.
5. announce as a builder note in-register; link goes in a reply, not the main post.
