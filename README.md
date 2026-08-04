<div align="center">

# Runbook

A field manual for troubleshooting: how to work out what is wrong, what to do first, and what to do when you are blocked.

[![Live][badge-site]][url-site]
[![HTML5][badge-html]][url-html]
[![CSS3][badge-css]][url-css]
[![JavaScript][badge-js]][url-js]
[![Claude Code][badge-claude]][url-claude]
[![License][badge-license]](LICENSE)

[badge-site]:    https://img.shields.io/badge/live_site-0063e5?style=for-the-badge&logo=googlechrome&logoColor=white
[badge-html]:    https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white
[badge-css]:     https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white
[badge-js]:      https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black
[badge-claude]:  https://img.shields.io/badge/Claude_Code-CC785C?style=for-the-badge&logo=anthropic&logoColor=white
[badge-license]: https://img.shields.io/badge/license-MIT-404040?style=for-the-badge

[url-site]:   https://runbook.neorgon.com/
[url-html]:   #
[url-css]:    #
[url-js]:     #
[url-claude]: https://claude.ai/code

</div>

---

## Overview

Troubleshooting is taught almost nowhere. Engineers are handed a pager and are expected to absorb the method by osmosis, usually during their first bad night. This is that method written down.

Nine chapters cover technique rather than any single technology: how to triage, how to find what changed, how to bisect, when to stop debugging and roll back, and how to escalate without wasting the next person's first twenty minutes. Each chapter ends with a worked example following a real failure from symptom to cause. An appendix covers twelve common alerts in reference form.

Every section is deep-linkable, the whole manual is searchable, and any chapter can be copied as Markdown into an incident channel or a runbook repository.

**Live:** runbook.neorgon.com

---

## Features

- **Nine chapters, 66 sections** covering triage, quick methods, long methods, escalation, and Plan B
- **Six worked examples** tracing real failures, including the wrong turns that cost time
- **Escalation checklists** by layer: infrastructure, development, dependencies, and everything else
- **Failure-mode reference** for twelve common alerts, each with the misdiagnosis that usually costs an hour
- **Search across every section**, filtered live from the sidebar
- **Copy any chapter as Markdown** for incident notes
- **Deep links** to any section, and a print stylesheet that paginates by chapter
- **Keyboard driven**: `/` search, `j`/`k` between sections, `?` shortcuts

---

## Running locally

```bash
make serve
```

Or manually:

```bash
python3 -m http.server 8844
```

ES modules require an HTTP server. Opening `index.html` over `file://` will not work.

---

## Architecture

Static, no-build ES modules. No npm, no bundler, no dependencies.

**Data flow:** `js/content/*.js` (chapter data) → `content/index.js` (aggregate) → `render.js` (DOM) ← `events.js` / `nav.js` (interaction)

```
incident-runbook-site/
├── index.html          # Shell: header, TOC container, doc container, footer
├── css/
│   ├── style.css       # Field-manual styles + Header Kit re-skin
│   ├── neorgon-header.css   # vendored, do not edit
│   ├── neorgon-themes.css   # vendored, do not edit
│   └── neorgon-footer.css   # vendored, do not edit
├── js/
│   ├── app.js          # Entry point
│   ├── content/        # One module per chapter, plus index.js aggregate
│   ├── render.js       # Block renderer + TOC builder
│   ├── nav.js          # Scroll spy, drawer
│   ├── events.js       # Search, copy, keyboard
│   ├── state.js        # Shared state + search corpus
│   ├── utils.js        # Escaping, inline markup, Markdown export
│   ├── neorgon-header.js    # vendored, do not edit
│   └── neorgon-footer.js    # vendored, do not edit
├── og-preview.jpg      # 1200×630 social preview
├── llms.txt
├── robots.txt
├── sitemap.xml
├── CNAME
├── Makefile
├── LICENSE
└── README.md
```

### Content model

Chapters are data, not markup. Each section holds a list of typed blocks rendered by `render.js`:

`p` · `ul` · `ol` · `defs` · `note` (callout) · `checklist` · `code` · `table` · `example`

Prose supports a small inline syntax, escaped before formatting: `**bold**`, `` `code` ``, and `[text](url)`.

Adding a chapter means one module in `js/content/`, plus an import and an entry in the `chapters` array in `js/content/index.js`. The table of contents, search index, and Markdown export all derive from that array.

### Shared kits

Header and footer come from `packages/neorgon-ui/` and are vendored here. **Never edit those files**. Edit the canonical source and re-run `sync-header.sh` / `sync-footer.sh`. The header is re-skinned through `--header-bg-*` token overrides in `style.css`, scoped to `:root:not([data-theme])` so visitor themes and the seasonal CDN default still win.

---

<div align="center">
<sub>Part of <a href="https://neorgon.com/">Neorgon</a></sub>
</div>
