# 4: Tech Spec

## Context

See `PRODUCT.md` for the problem and acceptance criteria. This is a documentation-only change: one link destination in `README.md`.

Current state of the affected file:

- `README.md:41` — the only scheme-less link, `[Warp Factory](warp.dev/factories)`, inside the "Warp Contributions Overview Dashboard" section. GitHub resolves the destination relative to the current page, producing a broken `github.com/warp.dev/factories` URL.
- `README.md:6`, `README.md:48`, `README.md:50` — the other factory links; all already use `https://warp.dev/factories` (line 50 also links `https://warp.dev/get-started`). These are correct and must not change.

## Files affected

- `README.md` — line 41 only.

No other files. No application code, Rust crates, dependencies, or generated files are involved, so no build or test runs are required for this change.

## Proposed change

Replace the link destination on `README.md:41`:

```diff path=null start=null
--- a/README.md
+++ b/README.md
@@ -38,7 +38,7 @@
 ## Warp Contributions Overview Dashboard
 
 Explore [build.warp.dev](https://build.warp.dev) to:
-- Watch thousands of [Warp Factory](warp.dev/factories) agents triage issues, write specs, implement changes, and review PRs
+- Watch thousands of [Warp Factory](https://warp.dev/factories) agents triage issues, write specs, implement changes, and review PRs
 - View top contributors and in-flight features
 - Track your own issues with GitHub sign-in
 - Click into active agent sessions in a web-compiled Warp terminal
```

Link text and all surrounding content are byte-identical before and after; the diff is exactly one changed line.

## Edge cases

- Keep the exact destination string `https://warp.dev/factories` to match lines 6, 48, and 50 — not `http://`, not `www.warp.dev/factories`.
- Do not "normalize" the other factory links while in the file; they already use the absolute HTTPS form.
- Verify no trailing-whitespace or line-ending drift is introduced on the edited line.

## Testing and validation

The repository has no docs-link lint, so validation is a targeted check plus render inspection:

1. **Static check** — confirm exactly one changed line in the diff (`git diff master -- README.md`), and that `grep -n '](warp.dev' README.md` returns no matches after the change (no scheme-less Markdown destinations remain in the README).
2. **Absolute-URL check** — `grep -n 'Warp Factory' README.md` shows the line 41 destination as `https://warp.dev/factories`.
3. **Render check** — view `README.md` on the PR's "Files changed" tab (GitHub renders Markdown there): hovering the "Warp Factory" link shows `https://warp.dev/factories`, and clicking it from the rendered README lands on the Warp Factories site, not `github.com/warp.dev/factories`.
4. **Scope check** — the PR's changed-files list contains only `README.md` (plus the spec files under `specs/4/`, which are part of this spec PR and not the implementation).

No `cargo` build, tests, or formatters apply — this change does not touch code. Manual testing via `./script/run` is not applicable to a Markdown link fix.
