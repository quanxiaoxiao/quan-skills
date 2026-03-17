# Migration Rules

This reference defines how to transform existing documentation into the layered governance model.

## Action Definitions

### `keep`
File is already in the correct layer and location. No changes needed.
- Verify content matches layer rules before marking as `keep`.

### `move`
Content is correct but the file is in the wrong location.
- Move the file to the target location.
- Update all internal cross-references.
- Leave a redirect comment in the old location if other docs reference it (remove after one audit cycle).

### `merge`
Multiple files cover the same topic and should be combined.
- Identify the authoritative file (prefer the one in the correct layer).
- Merge unique content from secondary files into the primary.
- Remove secondary files after merge.
- Resolve contradictions by choosing the more recent or more specific version; flag unresolvable conflicts for human review.

### `split`
One file spans multiple layers and should be separated.
- Identify which paragraphs/sections belong to which layer.
- Create new files in the correct layer locations.
- Replace split content in the original with cross-references.

### `rewrite`
Content is outdated, poorly structured, or does not follow layer rules.
- Rewrite in place if the file is in the correct location.
- Rewrite and move if the location is also wrong.
- Preserve factual content; improve structure and clarity.
- Flag uncertain facts for human review.

### `remove`
Content is duplicated elsewhere, obsolete, or does not belong in documentation.
- Verify the content truly exists elsewhere before removing.
- If content is unique but obsolete, confirm with the user before removing.

### `create`
A required document does not exist.
- Use the appropriate template from `templates/`.
- Fill only sections that have real content; leave others as explicit TODOs.
- Do not generate speculative documentation.

## Conflict Resolution

When two files contain contradictory information about the same topic:

1. Check git blame to find which was updated more recently.
2. Check if one file is in a higher-authority layer (Layer 3 > Layer 4 > Layer 2 for system knowledge).
3. If still ambiguous, flag for human review. Do not guess.

## Migration Order

Execute migrations in this order to minimize broken references:

1. `create` — establish target locations first
2. `move` — relocate correctly-structured content
3. `merge` — combine overlapping files
4. `split` — separate mixed-layer files
5. `rewrite` — update content in place
6. `remove` — clean up only after all moves and merges are done

## Safety Rules

- Never delete a file that is the only source of a piece of information.
- Always run the migration plan past the user before executing.
- Create a git commit after each migration phase for easy rollback.
- Keep a migration log: `Source → Action → Target → Status`.
