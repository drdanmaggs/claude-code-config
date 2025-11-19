# Design Token Philosophy

Universal principles for maintaining design consistency across all projects.

## Core Principle

**Design tokens are named variables representing design decisions, not implementation details.**

### ❌ Implementation-focused
```css
--color-pink-500: #ec4899;
--spacing-6: 1.5rem;
```

### ✅ Semantic/purpose-focused
```css
--color-brand-primary: #ec4899;
--spacing-comfortable: 1.5rem;
```

---

## Why Design Tokens Matter

### 1. Single Source of Truth
Change `--color-brand-primary` once, update 50 components instantly.

### 2. Consistent Visual Language
Prevents `p-4` next to `p-5` next to `p-[18px]` chaos.

### 3. Faster Development
Use `bg-brand-primary` instead of searching "what's our pink hex again?"

### 4. Easier Maintenance
Rebrand? Update tokens, not 500 hardcoded values.

### 5. Better Communication
Designers and developers speak same language: "brand primary" not "pink-500"

---

## The Semantic Naming System

### Hierarchy of Token Types

**1. Brand Foundation** (Rarely change)
```css
--color-brand-primary: #ec4899;
--color-brand-secondary: #8b5cf6;
--font-display: "Satoshi", sans-serif;
```

**2. Semantic UI** (UI states and meaning)
```css
--color-success: #10b981;
--color-error: #ef4444;
--color-text: #1f2937;
--color-text-subtle: #6b7280;
```

**3. Functional** (Component-specific)
```css
--color-surface: #ffffff;
--color-surface-elevated: #f9fafb;
--spacing-section: 4rem;
--radius-brand: 0.75rem;
```

### Naming Convention

Format: `--{category}-{purpose}[-variant]`

**Examples:**
- `--color-brand-primary` (category: color, purpose: brand-primary)
- `--spacing-comfortable` (category: spacing, purpose: comfortable)
- `--shadow-brand-glow` (category: shadow, purpose: brand-glow)

**NOT:**
- `--pink-500` (implementation detail)
- `--spacing-24px` (implementation detail)
- `--shadow-1` (meaningless number)

---

## Critical Rules

### Rule 1: Never Use Hardcoded Values

**❌ Forbidden:**
```tsx
<div className="p-4 mb-6 bg-[#ec4899]">
<div style={{padding: '24px', color: '#6b7280'}}>
```

**✅ Required:**
```tsx
<div className="p-comfortable mb-comfortable bg-brand-primary">
<div className="p-comfortable text-subtle">
```

### Rule 2: No Opacity Modifiers in Components

**❌ Forbidden:**
```tsx
<div className="bg-black/20 text-white/70 border-brand-primary/10">
```

**Why?** Opacity is a design decision, not a component decision. Define it once in your design system.

**✅ Required:**
Define tokens with opacity baked in:
```css
/* In globals.css */
@theme {
  --color-glass-dark: oklch(0 0 0 / 0.2);
  --color-text-muted: oklch(1 0 0 / 0.7);
  --color-border-subtle: oklch(0.627 0.265 303.9 / 0.1);
}
```

Then use semantic tokens:
```tsx
<div className="bg-glass-dark text-muted border-subtle">
```

### Rule 3: Design Tokens First, Utilities Second

**Decision framework:**
1. Does this value repeat across components? → Token
2. Is this a brand/design decision? → Token
3. Is this layout/spacing logic? → Utility is fine
4. Is this a one-off adjustment? → Arbitrary value OK (rarely)

**Examples:**
```tsx
<!-- Colors: ALWAYS tokens -->
<div className="bg-brand-primary text-white">

<!-- Layout: utilities fine -->
<div className="flex items-center justify-between">

<!-- Spacing: prefer tokens, utilities acceptable -->
<div className="p-comfortable"> <!-- Better -->
<div className="p-6">           <!-- Acceptable -->

<!-- One-off: last resort -->
<div className="mt-[13px]">     <!-- Only if truly unique -->
```

---

## Pragmatic Exceptions

### When to Break the Rules

**1. Fixed-context components**
```tsx
<!-- Navbar is ALWAYS over dark video background -->
<nav className="text-white">  <!-- Absolute colour OK -->
```

Why? The colour requirement is non-contextual and will never change.

**2. Layout utilities**
```tsx
<div className="flex items-center gap-4">  <!-- No tokens needed -->
```

Why? These are structural, not design decisions.

**3. Truly unique values**
```tsx
<div className="top-[73px]">  <!-- Specific offset for hero -->
```

Why? If it appears once and has a specific reason, arbitrary value OK.

---

## Implementation Patterns

### Starting a New Project

1. **Audit designs** for repeated values
2. **Extract tokens** (colours, spacing, typography)
3. **Define in globals.css** with semantic names
4. **Document** in project CLAUDE.md
5. **Enforce** in code reviews

### Token Organisation

```css
/* Recommended structure */
@theme {
  /* 1. Brand identity */
  --color-brand-primary: ...;
  --color-brand-secondary: ...;
  
  /* 2. Semantic UI */
  --color-success: ...;
  --color-error: ...;
  
  /* 3. Typography */
  --font-display: ...;
  --text-hero: ...;
  
  /* 4. Spacing */
  --spacing-tight: ...;
  --spacing-comfortable: ...;
  
  /* 5. Effects */
  --shadow-brand: ...;
  --radius-brand: ...;
}
```

---

## Cross-Project Consistency

All my projects follow these token patterns:

**Required tokens:**
- `--color-brand-primary`
- `--color-text` / `--color-text-subtle`
- `--spacing-comfortable`
- `--radius-brand`

**This enables:**
- Consistent user experience
- Faster development (muscle memory)
- Easier context switching between projects

---

## Common Mistakes

### 1. Over-tokenising
```css
/* ❌ Too granular */
--spacing-card-padding-top: 1.5rem;
--spacing-card-padding-bottom: 1.5rem;

/* ✅ Right level of abstraction */
--spacing-comfortable: 1.5rem;
```

### 2. Implementation-focused names
```css
/* ❌ What it is */
--color-pink-500: #ec4899;

/* ✅ What it means */
--color-brand-primary: #ec4899;
```

### 3. Duplicating Tailwind
```css
/* ❌ Redundant */
--spacing-4: 1rem;  /* Tailwind already has p-4 */

/* ✅ Semantic addition */
--spacing-section: 4rem;  /* Meaningful increment */
```

---

## The Bottom Line

**Design tokens are not about being pedantic—they're about:**
1. Making future changes easy
2. Maintaining consistency without thinking
3. Speaking a shared design language
4. Preventing "what was that hex code again?" moments

**Follow this guide for all projects, customise tokens per project.**
