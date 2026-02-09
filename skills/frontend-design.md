# Frontend Design

Create distinctive, production-grade frontend interfaces following our stack conventions.

## When to Use

- Building new UI components from scratch
- Designing landing pages or dashboards
- Implementing design systems
- When generic templates aren't acceptable

## Stack

**React**:
- React 19+ with TypeScript 
- TailwindCSS (utility-first, responsive)
- React Hook Form + Zod (forms)

**Angular** (when applicable):
- Standalone components (Angular 15+)
- Angular Material UI
- Reactive Forms
- OnPush change detection where appropriate

## Core Principles

1. **Semantic HTML** (`nav`, `main`, `article`, `section`, `header`, `footer`)
2. **Accessibility** (WCAG 2.1 AA minimum):
   - Keyboard navigation (focus states)
   - ARIA labels where needed
   - Color contrast: 4.5:1 for text, 3:1 for UI
   - Touch targets minimum 44x44px
3. **Responsive Design**:
   - Mobile-first approach
   - Breakpoints: 640px (sm), 768px (md), 1024px (lg), 1280px (xl)
4. **Component Composition**:
   - Small, reusable components
   - Props interfaces clearly defined
   - Variants for different states (default, hover, active, loading, error, disabled, empty)

## Output Format

When creating frontend designs:

### 1. Describe the Design Intention

```markdown
## Design: [Component/Page Name]

**Concept**: [Brief description of the design approach]
**Visual Style**: [Modern, minimal, bold, playful, etc.]
**Key Features**: [What makes this design distinctive]
```

### 2. Provide the Implementation

- Full TypeScript + React/Angular component code
- TailwindCSS for styling (or Angular Material for Angular)
- Proper prop types and interfaces
- Accessibility attributes included
- Responsive design baked in
- All states handled (loading, error, empty, disabled)

### 3. Include Usage Example

```tsx
// Example usage
<ComponentName prop1="value" prop2={true} />
```

### 4. Document Variants

- Default state
- Hover/active states
- Loading states
- Error states
- Disabled states
- Empty states

## Anti-Patterns to Avoid

- ❌ Generic blue buttons and white backgrounds (boring, overdone)
- ❌ Overly complex gradients (hard to maintain, distracting)
- ❌ Too many font families (inconsistent, unprofessional)
- ❌ Tiny touch targets on mobile (inaccessible)
- ❌ Missing loading/error states (poor UX)
- ❌ Hardcoded colors (not maintainable)
- ❌ Inline styles (defeats TailwindCSS benefits)
- ❌ Missing hover states (feels unpolished)
- ❌ Non-semantic HTML (bad for SEO and accessibility)
- ❌ Missing keyboard navigation (inaccessible)

## Deliverables

When using this skill, provide:

1. ✅ Complete, working code (no placeholders)
2. ✅ TypeScript types and interfaces
3. ✅ Responsive design (mobile to desktop)
4. ✅ Accessibility attributes
5. ✅ Loading/error/empty states
6. ✅ Usage examples
7. ✅ Design rationale (why these choices)

## Inspiration (Not Copying)

- Vercel's design (clean, modern, bold typography)
- Linear's interface (minimal, fast, gesture-driven)
- Stripe's dashboard (clear hierarchy, data-dense)
- Tailwind UI (well-composed, accessible)
- shadcn/ui (modern primitives, customizable)

**Remember**: Aim for distinctive, not derivative. Create designs that feel fresh and intentional.
