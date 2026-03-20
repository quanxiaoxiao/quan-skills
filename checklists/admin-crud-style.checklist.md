# Admin CRUD Style Checklist

Before completing a CRUD admin page, verify:

## Layout

- [ ] Page uses `p-6 space-y-6 bg-muted/30` container
- [ ] Page header has title and primary action button
- [ ] Filters are inside a Card
- [ ] Table is inside a separate Card
- [ ] Pagination appears under the table

## Components

- [ ] All required shadcn/ui components are installed
- [ ] Icons are from lucide-react only
- [ ] Status columns use Badge
- [ ] Delete uses AlertDialog with destructive style
- [ ] Forms use Dialog (simple) or Drawer (complex)

## Interaction

- [ ] Enter triggers search
- [ ] Reset clears all filters and resets pagination
- [ ] Edit opens dialog with pre-populated data
- [ ] Delete requires confirmation before executing
- [ ] Submit button disables during request and shows loading state
- [ ] Successful mutations show toast notification

## Code Organization

- [ ] Page, filters, table, form dialog are in separate files
- [ ] Custom hooks handle data fetching and mutations
- [ ] Hook naming follows `use<Entity>List` / `use<Entity>Form` pattern
- [ ] TypeScript types are defined for entity and form data

## States

- [ ] Loading state shows skeleton rows or overlay, not empty table
- [ ] Empty state shows clean message
- [ ] Error states are handled

## Accessibility

- [ ] Buttons have clear labels
- [ ] Form fields have proper labels
- [ ] Dialogs trap focus
- [ ] Keyboard navigation works
