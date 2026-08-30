# Open Questions (for a human, not for an agent to resolve solo)

This file exists so future Claude sessions don't lose track of unresolved calls. Add to it; don't silently resolve items here on your own judgment.

## 1. What should live in the vault vs. the repo?

**Status: unresolved, flagged 2026-08-25.**

Current de facto pattern (not a decided policy):
- Parachute vault = editable source of truth. Canonical notes get written/updated there first.
- This repo = a mirror of "governing reference" notes (vision, character, rig, performance architecture) as markdown, plus repo-only artifacts that don't really belong in a notes vault: single-file HTML pages, PNG posters/infographics, a Claude Design canvas link.

Open questions this doesn't answer:
- Should *every* canonical vault note get mirrored here, or only the ones actively needed for repo-based work (coding, HTML builds, design canvases)?
- When a vault note is updated, is re-syncing to the repo something that happens automatically-ish (a Claude session does it on request, like this one), or should it be a deliberate periodic pass?
- Should raw captures (`_captures/...` in the vault) ever live in the repo at all, or should the repo only ever hold synthesized/canonical material? (Current answer so far: yes, mirror them too, in `captures/`, clearly marked non-canonical — but this hasn't been explicitly decided, just done.)
- Posters/images (`images/*.png`) and their companion markdown docs exist only in the repo, not the vault (except where a vault note explicitly references the artifact, as in `docs/connection/two-wings.md`). Is that the right split, or should the vault also hold a record of visual artifacts?
- Is there a risk of drift — vault note updated, repo mirror stale — and if so, does the repo need a "last synced" marker per file?

## Next step

Adam and a future session should explicitly decide this rather than continuing to let precedent set the policy by accretion. Until then: treat the vault as authoritative on content, and treat any repo doc's provenance comment (`<!-- provenance: ... -->` at the top of each file) as the record of where and when it was last pulled from.
