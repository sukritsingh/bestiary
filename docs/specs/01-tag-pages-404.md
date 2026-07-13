# Fix: creature-type tag links return 404

## Problem
Clicking a creature type in the nav (e.g. "Humanoids"), or any non-CR tag on a
creature page, returns a 404.

The site now deploys via `.github/workflows/jekyll-gh-pages.yml`, which builds
with `actions/jekyll-build-pages@v1` (the `github-pages` gem). That build runs
Jekyll in safe mode and disables custom `_plugins/`, so `_plugins/tag_gen.rb`
never runs and no `tags/*.html` pages exist in the built site. (The old
`rake genpublish` local-build deploy did run the plugin, which is why the tags
used to work.)

## Fix (same pattern as grimoire spec 01)
The taggable space is small and fixed — 14 creature types, 6 sizes, `swarm`,
and 9 source books (30 tags total, enumerated from `_posts` front matter) — so
generate them as **static pages** instead of via a plugin:

- Add `tags/<tag>.html`, 4 lines of front matter each, using the existing
  `tag_index` layout:

  ```yaml
  ---
  layout: tag_index
  tag: humanoid
  title: Humanoids
  ---
  ```

- Delete `_plugins/tag_gen.rb` — dead code under the Actions build, and it
  would collide with the static pages when building locally.

CR tags keep linking to homepage anchors (`/#cr5`), unchanged — `post.html`
already special-cases tags containing `cr`.

## Also fixed while here
- `{% assign post_list = site.tags.[{{level.tag}}] ... %}` in `index.html` and
  `_layouts/tag_index.html` is invalid Liquid (nested `{{ }}` inside a tag) —
  it happened to work in lax mode but spammed build warnings. Now
  `site.tags[level.tag]`.
- `dread-warrior.markdown` was tagged `crl` (typo for `cr1`), so it never
  appeared in any CR listing. `avatar-of-death.markdown` is tagged `cr--`
  intentionally (its MM challenge rating is "—") and only shows on the by-name
  page.

## Trade-off
A creature with a brand-new tag (new source book, new type) needs one 4-line
file added under `tags/`. Acceptable for a fixed 5e taxonomy.
