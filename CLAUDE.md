# CLAUDE.md

Website for Gena Ramos, a professional hairdresser (cinema, TV, theatre, private clients and weddings). It replaces the old Google Sites page at genaramos.com. Built with Jekyll and served by GitHub Pages from `main`. Content is edited by the owner through Netlify CMS at `/admin/`. The site is in European Portuguese (pt-PT).

## Commands

- `bundle install`: install gems. Requires Ruby 2.7.6 (see `.ruby-version`, managed with rbenv).
- `bundle exec jekyll serve --livereload`: dev server on http://localhost:4000.
- `bundle exec jekyll build`: build into `_site/`.
- `bundle exec rake`: run html-proofer on `_site/` (build first).

## Constraints

- Built by **GitHub Pages**, so only whitelisted plugins work and Jekyll is pinned to 3.9 by the `github-pages` gem. Don't add plugins or Jekyll 4 features.
- `Gemfile.lock` and `_site/` are gitignored.
- `_config.yml` changes need a server restart.
- GitHub Pages compiles Sass with Ruby Sass, so avoid CSS `min()`/`max()`.

## Content

- Netlify CMS (`admin/config.yml`, editorial workflow) opens PRs from `cms/...` branches. If you rename front-matter fields, data keys or files, update `admin/config.yml` to match. The CMS UI is in Portuguese (`locale: 'pt'`); keep field labels in Portuguese.
- Page copy is front matter: `index.md` (home, layout `home`), `sobre.md` (`about`), `servicos.md` (`services`), `portfolio.md` (`portfolio`), `contactos.md` (`contact`). Permalinks match the old site (`/servicos/`, `/portfolio/`, `/contactos/`) plus `/sobre/`. Titles split into `headline` and an optional `headline_accent` (rendered gold and italic). Pages end with the gold contact band, set by `band_title` and `band_text`.
- Structured content is in `_data/`: `settings.yml` (name, phone, email, links), `services.yml`, `portfolio.yml` (categories with credits; `featured: true` shows a credit on the home page). Never hard-code the phone or email in layouts.
- Media goes in `assets/uploads/`. Logos and favicons are in `assets/images/`.

## Design

- The design source is `pen/genaramos.pen` (open it with the Pencil MCP tools, never read it directly). The chosen direction is `A · Noir` (a reusable frame); `A · Noir (claro)` is an instance of it with `theme: {mode: light}`, so edit A and both themes follow. Inner pages `Sobre`, `Serviços`, `Portfólio` and `Contactos` follow the same pattern (reusable dark frame plus a `(claro)` instance). Shared components: `Site Nav` (override the active link's fill to `$accent` per page), `Site Footer` and `Theme Switcher` (variants `selection: system | light | dark`). Components must not set their own `theme`, or they won't follow the page. The `Design System` board holds the palette, the semantic tokens and the switcher spec. `B · Atelier`, `C · Créditos` and `D · Ouro` are rejected alternatives kept for reference.
- Colours in A must use the semantic, themed variables (`bg`, `surface`, `text`, `muted`, `subtle`, `rule`, `rule-strong`, `accent`, `accent-fill`, `on-accent`), never palette steps or hex values directly. Gold text on light backgrounds is `gold-700` (`#FECE45` fails contrast on white); `gold-400` is only for fills there.
- Themes: `_sass/_tokens.scss` defines dark tokens on `:root`, light tokens under `prefers-color-scheme: light` (unless `data-theme="dark"`), and light again for `[data-theme="light"]`. An inline script in `<head>` of `_layouts/default.html` sets `data-theme` from localStorage before paint; the switcher script at the end of that layout stores or clears the choice (no stored value = Sistema). Both logo versions are in the markup and CSS shows one per theme (`--logo-dark` / `--logo-light`).
- Icons are Lucide 0.460.0 SVGs copied into `_includes/icons/`; use `{% include icon.html name="sun" %}`. To add one, copy it from the `lucide-static` package the same way (class `icon`, no width/height).
- Styles: `_sass/_base.scss` (tokens in use, buttons, band, credit rows), `_header.scss`, `_home.scss`, `_pages.scss` (Sobre, Serviços, Portfólio, Contactos), `_footer.scss`. Avoid Liquid `page[a[1]]` nested lookups; assign the key first.
- Brand colours come from the logo (`logo-design/`): gold `#FECE45`, ink `#141414`, navy `#000061`, white. Tokens are CSS variables in `_sass/_tokens.scss` and match the variables in the .pen file.
- The logo keeps its gold scissors in both themes: gold/white on dark, gold/black on light (recoloured from `logo-design/Logo-02.svg`). The navy/black originals are not used on the site.
- Only the one-page public CV is published, as `assets/uploads/cv-gena-ramos.pdf` (source: `pen/CV - Gena Ramos.pdf`). `pen/CV - Gena Ramos - full.pdf` holds private details: it is gitignored and must never be copied into the site or committed. `_data/portfolio.yml` follows the full CV's credit list, with theatre credits grouped by `venue`.
- "Vai de Mail a Pior" is spelled that way on purpose (a pun on email). Don't correct it.
- `pen/images/` holds the design's images; `pen/images/old/` is the raw download of every image from the old site; it is gitignored and exists only locally.
