# Admin CRUD Scaffold

> Authoritative prompt: `quan-skills:prompts/admin-crud-scaffold.md`
> Authoritative skill: `quan-skills:skills/admin-crud-style/SKILL.md`
> Related checklist: `quan-skills:checklists/admin-crud-style.checklist.md`

## Task

Scaffold a complete CRUD admin page for a given entity using React + shadcn/ui + Tailwind + lucide-react.

## Inputs

- Entity name (e.g., `user`, `product`, `order`)
- Fields list with types (filterable, table-visible, form-editable)
- API endpoint pattern (e.g., `/api/users`)

## Steps

1. Check that required shadcn/ui components are installed. Install any missing:
   `npx shadcn add card button input select table badge dialog alert-dialog dropdown-menu skeleton label pagination tooltip`

2. Create the page structure following the standard layout:
   - `pages/<entity>/index.tsx` — main page with header, filter card, table card
   - `pages/<entity>/components/<entity>-filters.tsx` — filter form
   - `pages/<entity>/components/<entity>-table.tsx` — data table with columns
   - `pages/<entity>/components/<entity>-form-dialog.tsx` — create/edit dialog
   - `pages/<entity>/hooks/use-<entity>-list.ts` — list query + pagination + filters
   - `pages/<entity>/hooks/use-<entity>-form.ts` — create/update/delete mutations

3. Implement standard interaction patterns:
   - Enter triggers search
   - Reset clears filters and resets pagination to page 1
   - Edit opens dialog pre-populated with row data
   - Delete shows AlertDialog confirmation
   - Form submit disables button and shows loading state

4. Apply consistent styling:
   - Page: `p-6 space-y-6 bg-muted/30`
   - Cards: `p-4`
   - Spacing: `gap-4` within sections
   - Corners: `rounded-xl`

## Output

- List of files created
- shadcn components installed
- Verification that layout and interactions match the skill spec
