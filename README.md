# undelete-legal

Public legal and support pages for **Undelete - Restore Deleted Issues for Jira**, an
Atlassian Marketplace app. Served by GitHub Pages.

This repo is public **only** so the Marketplace listing can point at stable URLs. The app
source lives elsewhere and stays private.

## URLs the Marketplace listing needs

| Listing field | URL |
|---|---|
| Privacy policy (mandatory) | `/privacy` |
| Documentation | `/docs` |
| Support | `mailto:` the address in `_config.yml` |

## Editing

Identity — legal name, business ID, address, support email, effective date — lives in
`_config.yml` and nowhere else. The pages read it as `{{ site.* }}`. Change it once,
there.

Page content is plain Markdown rendered by `_layouts/default.html`. No theme gem, no
plugins, no Gemfile — GitHub Pages builds it as-is.

## Keeping it honest

`privacy.md` mirrors `docs/listing/data-handling.md` in the private app repo, which is the
source of record for every technical claim in it. **If capture behaviour or retention
changes in the app, both files change.** A privacy policy that has drifted from the code
is worse than none.

`limitations.md` and `docs.md` mirror `docs/listing/known-limitations.md` and
`docs/listing/DOCS-STE.md` the same way.
