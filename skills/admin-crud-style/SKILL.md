name: admin-crud-style
description: Unified CRUD admin page style using React + shadcn/ui + Tailwind + lucide-react. Designed for management systems with filters, tables, pagination, and add/edit/delete operations.

---

# Admin CRUD Style Skill

## Purpose

This skill enforces a **consistent UI and code structure** for CRUD-style admin pages.

Most management pages follow the same pattern:

- filter / search area
- data table
- pagination
- add / edit / delete actions
- modal or drawer forms

This skill ensures that all generated pages follow the same layout, style, interaction patterns, and component usage.

The goal is to produce **clean, stable, maintainable admin interfaces** with consistent UX.

---

# Technology Stack

All generated code must use:

- React
- Tailwind CSS
- shadcn/ui components
- lucide-react icons

Do not introduce other UI frameworks.

---

# Global Design Philosophy

Admin pages must follow these principles:

- clarity over decoration
- functional business UI
- consistent layout across all pages
- minimal visual noise
- predictable interaction patterns
- reusable components

Avoid:

- marketing-style layouts
- heavy animations
- unnecessary visual effects
- inconsistent spacing or typography

---

# Page Layout Structure

Every CRUD list page must follow this structure.

```
PageContainer
├─ PageHeader
│   ├─ Title
│   └─ Primary Action (Add button)
│
├─ FilterCard
│   └─ Filter form
│
├─ TableCard
│   ├─ Data Table
│   └─ Pagination
│
└─ FormDialog or FormDrawer
    └─ Add / Edit Form
```

Rules:

- filters and table must be placed inside separate Cards
- pagination always appears under the table
- add/edit forms must use Dialog or Drawer
- page header must appear at the top

---

# Page Container Rules

Use consistent spacing for all pages.

Recommended structure:

- page padding: `p-6`
- section spacing: `gap-6`
- card padding: `p-4` or `p-6`

Page background should be light neutral.

Example:

```
bg-muted/30
```

---

# Page Header Rules

The header contains:

- page title
- optional description
- primary action button

Example layout:

```
Title              [ Add Button ]
Description
```

Rules:

- title must be concise
- primary action appears on the right
- primary action is usually "Add"

---

# Filter Area Rules

Filters must appear inside a Card.

Example structure:

```
Card
└─ Filter Row
   ├─ Search Input
   ├─ Select
   ├─ Status Filter
   ├─ Date Filter
   └─ Action Buttons
```

Rules:

- maximum 4 filter fields per row
- keep filter area compact
- search input should be prominent
- query and reset buttons must be on the right
- pressing Enter triggers search
- reset clears filters and resets pagination

Spacing:

```
gap-4
```

---

# Table Area Rules

Tables must always be placed inside a Card.

Structure:

```
Card
├─ Table
└─ Pagination
```

Rules:

- consistent row height
- header text slightly emphasized
- action column fixed to the right
- numeric values right aligned
- text values left aligned

---

# Table Column Rules

Common column behaviors:

### Status

Use Badge component.

Example:

```
<Badge variant="success">Active</Badge>
```

---

### Long Text

Use truncation.

```
truncate max-w-[200px]
```

Add tooltip when necessary.

---

### Date Fields

Use consistent formatting.

Example:

```
YYYY-MM-DD
YYYY-MM-DD HH:mm
```

---

### Action Column

Action column must contain:

- Edit
- Delete

Example:

```
Edit | Delete
```

Rules:

- delete action must require confirmation
- destructive actions must use destructive style

---

# Table Loading State

When data is loading:

Use one of the following:

- skeleton rows
- loading overlay

Do not display empty table during loading.

---

# Empty State

When no data exists:

Show a clean empty state.

Example:

```
No data available
Try adjusting your filters
```

Avoid large illustrations.

---

# Pagination Rules

Pagination must always appear under the table.

Behavior rules:

- changing page preserves filters
- page reset occurs when filters change
- display total count

---

# Form Rules

Forms are used for:

- create
- update

Two container types:

### Dialog

Use when:

- simple form
- few fields

---

### Drawer

Use when:

- complex form
- many fields
- multi-section forms

---

# Form Layout Rules

Field layout should be consistent.

Guidelines:

Small forms:

```
single column
```

Medium forms:

```
two columns
```

Field spacing:

```
gap-4
```

Label alignment must be consistent.

---

# Form Actions

Form footer must include:

```
Cancel | Submit
```

Submit behavior:

- disable during request
- show loading state
- prevent duplicate submissions

---

# Delete Confirmation

Delete actions must show confirmation dialog.

Use:

```
AlertDialog
```

Example message:

```
Are you sure you want to delete this item?
This action cannot be undone.
```

---

# Icons

Icons must come from:

```
lucide-react
```

Icons should be subtle.

Examples:

```
Plus
Search
Edit
Trash
MoreHorizontal
```

Avoid excessive icon usage.

---

# Component Usage Rules

Prefer these shadcn components:

- Card
- Button
- Input
- Select
- Table
- Badge
- Dialog
- Drawer
- AlertDialog
- DropdownMenu
- Skeleton

Avoid introducing unnecessary custom UI components.

---

# Code Organization Rules

Pages should be broken into logical parts.

Example structure:

```
users-page.tsx
users-filters.tsx
users-table-columns.tsx
users-form-dialog.tsx
```

Avoid placing all logic in a single file.

---

# Hook Naming

Hooks should follow this style:

```
useUsersList
useUserForm
useUsersFilters
```

---

# File Naming Conventions

Use consistent naming.

Examples:

```
users-page.tsx
users-form-dialog.tsx
users-table-columns.ts
users-filters.tsx
```

---

# Interaction Behavior

The following behaviors must be consistent:

Search

- pressing Enter triggers search

Reset

- clears filters
- resets pagination

Edit

- opens dialog with populated data

Delete

- requires confirmation

Create

- opens empty form dialog

---

# Styling Rules

Use Tailwind utility classes.

Preferred spacing scale:

```
gap-4
gap-6
p-4
p-6
```

Border style:

```
border-muted
```

Rounded corners:

```
rounded-xl
```

Avoid excessive shadows.

---

# Performance Considerations

For large tables:

- support server-side pagination
- avoid rendering excessive rows

---

# Accessibility

Ensure:

- buttons have clear labels
- forms have proper labels
- dialogs trap focus
- keyboard navigation works

---

# Output Expectations

Generated admin pages must:

- follow the same page structure
- reuse consistent layout patterns
- maintain predictable interaction behavior
- produce maintainable code
- align visually with other admin pages

---

# Required shadcn/ui Components

When creating CRUD pages, ensure these shadcn/ui components are available. Install them if missing:

```bash
npx shadcn add card
npx shadcn add button
npx shadcn add input
npx shadcn add select
npx shadcn add table
npx shadcn add badge
npx shadcn add dialog
npx shadcn add drawer
npx shadcn add alert-dialog
npx shadcn add dropdown-menu
npx shadcn add skeleton
npx shadcn add label
npx shadcn add pagination
npx shadcn add tooltip
```

---

# Code Templates

## Template 1: Page Structure

```tsx
import { Plus } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";

export default function UsersPage() {
  return (
    <div className="p-6 space-y-6 bg-muted/30 min-h-screen">
      {/* Page Header */}
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-semibold tracking-tight">Users</h1>
          <p className="text-muted-foreground text-sm mt-1">
            Manage user accounts and permissions
          </p>
        </div>
        <Button>
          <Plus className="mr-2 h-4 w-4" />
          Add User
        </Button>
      </div>

      {/* Filters */}
      <Card className="p-4">
        <UserFilters />
      </Card>

      {/* Table */}
      <Card className="p-4">
        <UsersTable />
        <UsersPagination />
      </Card>

      {/* Form Dialog */}
      <UserFormDialog />
    </div>
  );
}
```

## Template 2: Filter Component

```tsx
import { Search } from "lucide-react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { useState } from "react";

export function UserFilters() {
  const [search, setSearch] = useState("");

  const handleSearch = () => {
    // Trigger search with current filters
  };

  const handleReset = () => {
    setSearch("");
    // Reset pagination and trigger search
  };

  return (
    <div className="flex flex-wrap items-end gap-4">
      <div className="flex-1 min-w-[200px]">
        <Input
          placeholder="Search users..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          onKeyDown={(e) => e.key === "Enter" && handleSearch()}
          className="w-full"
        />
      </div>
      {/* Add more filters here */}
      <div className="flex gap-2 ml-auto">
        <Button variant="outline" onClick={handleReset}>
          Reset
        </Button>
        <Button onClick={handleSearch}>
          <Search className="mr-2 h-4 w-4" />
          Search
        </Button>
      </div>
    </div>
  );
}
```

## Template 3: Table Component

```tsx
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { Edit, Trash } from "lucide-react";

interface User {
  id: string;
  name: string;
  email: string;
  status: "active" | "inactive";
  createdAt: string;
}

interface UsersTableProps {
  data: User[];
  loading?: boolean;
  onEdit: (user: User) => void;
  onDelete: (user: User) => void;
}

export function UsersTable({ data, loading, onEdit, onDelete }: UsersTableProps) {
  if (loading) {
    return <UsersTableSkeleton />;
  }

  if (data.length === 0) {
    return (
      <div className="text-center py-12 text-muted-foreground">
        <p>No data available</p>
        <p className="text-sm">Try adjusting your filters</p>
      </div>
    );
  }

  return (
    <Table>
      <TableHeader>
        <TableRow>
          <TableHead>Name</TableHead>
          <TableHead>Email</TableHead>
          <TableHead>Status</TableHead>
          <TableHead>Created</TableHead>
          <TableHead className="text-right">Actions</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {data.map((user) => (
          <TableRow key={user.id}>
            <TableCell className="font-medium">{user.name}</TableCell>
            <TableCell>{user.email}</TableCell>
            <TableCell>
              <Badge variant={user.status === "active" ? "default" : "secondary"}>
                {user.status}
              </Badge>
            </TableCell>
            <TableCell>{user.createdAt}</TableCell>
            <TableCell className="text-right">
              <div className="flex justify-end gap-2">
                <Button
                  variant="ghost"
                  size="icon"
                  onClick={() => onEdit(user)}
                >
                  <Edit className="h-4 w-4" />
                </Button>
                <Button
                  variant="ghost"
                  size="icon"
                  className="text-destructive hover:text-destructive"
                  onClick={() => onDelete(user)}
                >
                  <Trash className="h-4 w-4" />
                </Button>
              </div>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

## Template 4: Form Dialog

```tsx
import { useState } from "react";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogFooter,
} from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

interface UserFormDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  initialData?: User | null;
  onSubmit: (data: UserFormData) => void;
  loading?: boolean;
}

export function UserFormDialog({
  open,
  onOpenChange,
  initialData,
  onSubmit,
  loading,
}: UserFormDialogProps) {
  const [formData, setFormData] = useState<UserFormData>(() =>
    initialData
      ? { name: initialData.name, email: initialData.email }
      : { name: "", email: "" }
  );

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSubmit(formData);
  };

  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="sm:max-w-[500px]">
        <DialogHeader>
          <DialogTitle>
            {initialData ? "Edit User" : "Add User"}
          </DialogTitle>
        </DialogHeader>
        <form onSubmit={handleSubmit}>
          <div className="grid gap-4 py-4">
            <div className="space-y-2">
              <Label htmlFor="name">Name</Label>
              <Input
                id="name"
                value={formData.name}
                onChange={(e) =>
                  setFormData((prev) => ({ ...prev, name: e.target.value }))
                }
                placeholder="Enter name"
              />
            </div>
            <div className="space-y-2">
              <Label htmlFor="email">Email</Label>
              <Input
                id="email"
                type="email"
                value={formData.email}
                onChange={(e) =>
                  setFormData((prev) => ({ ...prev, email: e.target.value }))
                }
                placeholder="Enter email"
              />
            </div>
          </div>
          <DialogFooter>
            <Button
              type="button"
              variant="outline"
              onClick={() => onOpenChange(false)}
              disabled={loading}
            >
              Cancel
            </Button>
            <Button type="submit" disabled={loading}>
              {loading ? "Saving..." : "Save"}
            </Button>
          </DialogFooter>
        </form>
      </DialogContent>
    </Dialog>
  );
}
```

## Template 5: Delete Confirmation

```tsx
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
} from "@/components/ui/alert-dialog";

interface DeleteConfirmDialogProps {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  onConfirm: () => void;
  title?: string;
  description?: string;
  loading?: boolean;
}

export function DeleteConfirmDialog({
  open,
  onOpenChange,
  onConfirm,
  title = "Are you sure?",
  description = "This action cannot be undone.",
  loading,
}: DeleteConfirmDialogProps) {
  return (
    <AlertDialog open={open} onOpenChange={onOpenChange}>
      <AlertDialogContent>
        <AlertDialogHeader>
          <AlertDialogTitle>{title}</AlertDialogTitle>
          <AlertDialogDescription>{description}</AlertDialogDescription>
        </AlertDialogHeader>
        <AlertDialogFooter>
          <AlertDialogCancel disabled={loading}>Cancel</AlertDialogCancel>
          <AlertDialogAction
            onClick={onConfirm}
            disabled={loading}
            className="bg-destructive text-destructive-foreground hover:bg-destructive/90"
          >
            {loading ? "Deleting..." : "Delete"}
          </AlertDialogAction>
        </AlertDialogFooter>
      </AlertDialogContent>
    </AlertDialog>
  );
}
```

## Template 6: Custom Hook

```tsx
import { useState, useCallback } from "react";
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";

interface UseUsersListOptions {
  pageSize?: number;
}

export function useUsersList(options: UseUsersListOptions = {}) {
  const { pageSize = 10 } = options;
  const queryClient = useQueryClient();
  
  const [page, setPage] = useState(1);
  const [filters, setFilters] = useState({ search: "" });

  // Query for fetching data
  const { data, isLoading, error } = useQuery({
    queryKey: ["users", page, filters],
    queryFn: async () => {
      // Replace with actual API call
      const response = await fetch(`/api/users?page=${page}&search=${filters.search}`);
      return response.json();
    },
  });

  // Reset page when filters change
  const updateFilters = useCallback((newFilters: Partial<typeof filters>) => {
    setFilters((prev) => ({ ...prev, ...newFilters }));
    setPage(1); // Reset to first page
  }, []);

  const resetFilters = useCallback(() => {
    setFilters({ search: "" });
    setPage(1);
  }, []);

  return {
    data,
    isLoading,
    error,
    page,
    setPage,
    filters,
    updateFilters,
    resetFilters,
  };
}

export function useUserForm() {
  const queryClient = useQueryClient();

  const createMutation = useMutation({
    mutationFn: async (data: UserFormData) => {
      const response = await fetch("/api/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
      });
      return response.json();
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });

  const updateMutation = useMutation({
    mutationFn: async ({ id, data }: { id: string; data: UserFormData }) => {
      const response = await fetch(`/api/users/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
      });
      return response.json();
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });

  const deleteMutation = useMutation({
    mutationFn: async (id: string) => {
      const response = await fetch(`/api/users/${id}`, {
        method: "DELETE",
      });
      return response.json();
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });

  return {
    create: createMutation.mutateAsync,
    update: updateMutation.mutateAsync,
    delete: deleteMutation.mutateAsync,
    isCreating: createMutation.isPending,
    isUpdating: updateMutation.isPending,
    isDeleting: deleteMutation.isPending,
  };
}
```

---

# Implementation Checklist

When creating a new CRUD admin page:

- [ ] Install required shadcn/ui components
- [ ] Create page component following the structure template
- [ ] Create filter component with search, reset functionality
- [ ] Create table component with loading and empty states
- [ ] Create form dialog (or drawer for complex forms)
- [ ] Create delete confirmation dialog
- [ ] Create custom hooks for data fetching and mutations
- [ ] Implement pagination logic
- [ ] Add proper TypeScript types
- [ ] Ensure all interactions follow the behavior rules
- [ ] Test keyboard navigation and accessibility

---

# File Structure Example

```
pages/
├── users/
│   ├── index.tsx              # Main page
│   ├── components/
│   │   ├── user-filters.tsx   # Filter component
│   │   ├── users-table.tsx    # Table component
│   │   └── user-form-dialog.tsx
│   └── hooks/
│       ├── use-users-list.ts
│       └── use-user-form.ts
└── products/
    ├── index.tsx
    ├── components/
    │   ├── product-filters.tsx
    │   ├── products-table.tsx
    │   └── product-form-dialog.tsx
    └── hooks/
        ├── use-products-list.ts
        └── use-product-form.ts
```

---

# Tailwind CSS Configuration Notes

For Tailwind CSS v4 (as used in this project):

```css
/* index.css */
@import "tailwindcss";

@theme {
  /* Your theme customizations */
}
```

Ensure `bg-muted/30` works by having the muted color defined in your theme.

---

# React Query Integration

This project uses @tanstack/react-query. Follow these patterns:

1. Use `useQuery` for data fetching
2. Use `useMutation` for create/update/delete
3. Invalidate queries after mutations:
   ```ts
   queryClient.invalidateQueries({ queryKey: ["users"] });
   ```
4. Use query keys consistently

---

# Form Validation

This project uses Zod for validation. Example:

```ts
import { z } from "zod";

const userSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email"),
});

type UserFormData = z.infer<typeof userSchema>;
```

Integrate with forms using controlled components.

---

# Toast Notifications

This project uses `sonner` for toast notifications:

```tsx
import { toast } from "sonner";

// Success
toast.success("User created successfully");

// Error
toast.error("Failed to create user");
```

Use after mutations complete.
