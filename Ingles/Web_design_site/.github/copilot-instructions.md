# Copilot / AI agent instructions — Web_design_site

Quick context
- Small static multi-page website in Portuguese (pt-br).
- No build system, package manager, or tests in repo.
- Primary content lives under `Megaman_x6_guide/` (HTML/CSS/images). Root has a placeholder `translation.js`.

Where to start (quick tasks)
1. Open `Megaman_x6_guide/guia.html` — this is the main entry page.
2. Shared layout/styles are in `Megaman_x6_guide/bone.css` (base/header/footer). Page-specific styles are `*.css` (e.g., `dicas.css`, `detalhes.css`).
3. Images and static assets are in `Megaman_x6_guide/imagens/` and referenced with relative paths (e.g., `imagens/logo.png`).

Big picture (architecture & decisions)
- Static, serverless site: each page is a standalone HTML file that includes `bone.css` plus a page-specific stylesheet. There is no bundler/runtime detection; assume files are served as static assets.
- Repeated layout: header + nav + footer are duplicated across pages (no template engine detected). When refactoring, prefer minimal invasive edits (keeping relative paths intact).
- `translation.js` exists but is empty. It appears intended as a place for client-side translation strings, not yet integrated.

Patterns & conventions (codebase-specific)
- CSS ordering matters: `bone.css` first, followed by page CSS; page CSS overrides base rules. Example: `guia.html` loads `guia.css` then `bone.css` (note: this project sometimes loads `bone.css` then page-specific; keep the existing order when adding styles).
- Semantic HTML structure is used (`header`, `main`, `article`, `section`, `footer`). Use these elements to locate content areas.
- Content language is Portuguese — preserve language in existing text; if adding translations, document where strings live (see Translation notes).
- Navigation uses relative page links (e.g., `dicas.html`). When renaming/moving pages, update these links.

Translation notes
- `translation.js` is a discovered placeholder (root file with `const textos = {}`). If implementing translations, keep strings in that file and add a small loader script to swap text nodes by `id` or `data-i18n` attributes.
- Example (to add in `translation.js`):

```javascript
const textos = {
  pt: { title: 'Guia Mega Man X6' },
  en: { title: 'Mega Man X6 Guide' }
}
// Small function to apply
function applyLang(lang) {
  document.querySelectorAll('[data-i18n]').forEach(el => {
    const key = el.getAttribute('data-i18n')
    el.textContent = textos[lang][key] || el.textContent
  })
}
```

Developer workflow (what to expect)
- No build/test commands to run — preview by opening HTML files in a browser or use a local static preview (e.g., VS Code Live Server) for live reload.
- Debugging: use browser DevTools for CSS/layout issues; images are referenced relative to the page location.

Integration and external dependencies
- No external services or libraries detected in repo. All assets are local.

Safety & preservation
- Many pages contain textual content in Portuguese; avoid changing copy without request. Preserve original file structure and relative paths when moving or refactoring.

If you edit or refactor
- Keep `bone.css` as the shared source of truth for header/footer styles.
- Update nav links in every HTML file when adding/removing pages.
- When adding JavaScript, include it via `<script src="..."></script>` at the end of `body` so it runs after DOM is parsed.

Questions for the maintainer (ask before making these changes)
- Do you want translations implemented in `translation.js` or separate localized HTML files?
- Should we consolidate the repeated header/footer into an include/template system (requires adding tooling) or keep the simple static files approach?

If anything above is unclear or incomplete, tell me which areas you want expanded or examples added (e.g., specific refactor steps, translation implementation, or a PR checklist).