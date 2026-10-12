# 4: Fix the README factory link

## Summary

In `README.md`, the "Warp Contributions Overview Dashboard" section links the text "Warp Factory" to `warp.dev/factories`. Because the destination has no URL scheme, GitHub renders it as a relative URL on `github.com` instead of a link to the Warp Factories site. The fix is to make that one link destination absolute: `https://warp.dev/factories`.

## Problem

`README.md:41` currently reads:

```markdown path=/workspace/warp-stanford/README.md start=41
- Watch thousands of [Warp Factory](warp.dev/factories) agents triage issues, write specs, implement changes, and review PRs
```

A scheme-less Markdown destination like `warp.dev/factories` is resolved relative to the current page, so a reader clicking "Warp Factory" from the README lands on `https://github.com/warp.dev/factories`, which does not exist. Every other factory link in the README — lines 6, 48, and 50 — already uses the absolute form `https://warp.dev/factories`, so line 41 is the sole outlier.

## Requested change

Change the link destination on `README.md:41` from `warp.dev/factories` to `https://warp.dev/factories`. The link text, the surrounding bullet, and all other README content stay unchanged. The diff is a single line.

## Acceptance criteria

1. The "Warp Factory" link in the "Warp Contributions Overview Dashboard" section of `README.md` points to `https://warp.dev/factories` (absolute HTTPS URL).
2. Rendered on GitHub, the link navigates to `https://warp.dev/factories`, not a relative `github.com` path.
3. No other line of `README.md` changes; no application code, dependencies (`Cargo.toml`/lockfiles), or unrelated documentation files are touched.
4. The change is delivered as a draft pull request and is not merged (this issue doubles as a smoke test of the factory's student-request workflow).

## Out of scope

- Changing any other README link, wording, or structure (including lines 6, 48, and 50, which are already correct).
- Fixing scheme-less links anywhere else in the repository.
- Any application code, build, or dependency changes.

## Open questions

None. Triage verified the one-line root cause and found no duplicates; the requested change is unambiguous.
