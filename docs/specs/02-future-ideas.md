# Future ideas (lightweight, no new dependencies)

Candidates that stay within the current static-Jekyll + vanilla-JS setup.
None are implemented yet.

1. **Size tag pages in the nav.** `tags/tiny.html` … `tags/gargantuan.html`
   already exist after spec 01; adding a second row to `header.html` looping
   over `site.data.sizes` is ~4 lines.
2. **Source-book index.** Same idea: a `_data/books.yml` and a nav row (or a
   list on the About page) linking the 9 existing book tag pages.
3. **Search across tag pages.** `tag_index.html` already includes the Jets
   search box; verify the `jetsHide`/`jetsContent` hookup matches the homepage
   so filtering works there too.
4. **Print-friendly stat blocks.** A short `@media print` block in
   `_sass/_layout.scss` hiding header/footer/search — useful at the table.
5. **"Random creature" link.** ~10 lines of inline JS picking a random URL
   from a Liquid-emitted array of post URLs.

Deliberately avoided: JSON data files, client-side frameworks, CR calculators,
or anything requiring a build-step change — the value of this site is that a
new creature is one markdown file.
