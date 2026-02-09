# Frontend Review

Comprehensive frontend code review for React and Angular applications.

## When to Use

- After implementing new UI components
- Before creating pull requests
- When reviewing teammate's frontend code
- After major refactoring
- When accessibility compliance is required

## Review Checklist

### 1. React & Hooks

- Are hooks used correctly? (rules of hooks, dependency arrays complete and minimal)
- Are components properly memoized when needed? (`React.memo`, `useMemo`, `useCallback`)
- Is state management appropriate? (local vs global, avoid prop drilling beyond 2 levels)
- Are effects cleaned up properly? (return cleanup function from `useEffect`)
- Are refs used correctly? (not for state that triggers re-renders)

### 2. Angular-Specific (when applicable)

- Are standalone components used? (Angular 15+, no NgModules)
- Is OnPush change detection used where appropriate?
- Are reactive forms properly validated?
- Is RxJS used correctly?
  - Unsubscribe in `ngOnDestroy` or use `async` pipe
  - Avoid nested subscribes
  - Use operators (`map`, `switchMap`, `combineLatest`) instead of imperative logic
- Are services provided at correct level? (`providedIn: 'root'` vs component-level)
- Is `trackBy` used in `*ngFor` loops?

### 3. TypeScript

- Are types properly defined? (no `any` without justification)
- Are props interfaces clear and complete?
- Are union types used where appropriate?
- Are generics used correctly?
- Is type inference leveraged properly? (avoid redundant type annotations)

### 4. Performance

- Are expensive operations memoized? (`useMemo`, `useCallback` in React)
- Are lists properly keyed? (unique, stable keys—not array indexes)
- Are components split appropriately to avoid unnecessary re-renders?
- Are images optimized? (lazy loading, proper formats: WebP/AVIF)
- Are bundle sizes reasonable? (check for large dependencies)

### 5. Accessibility (WCAG 2.1 AA)

- Are interactive elements keyboard accessible? (tab order, Enter/Space handlers)
- Do all images have alt text? (empty `alt=""` for decorative images)
- Are color contrasts sufficient? (4.5:1 for text, 3:1 for UI)
- Are ARIA labels used correctly? (`aria-label`, `aria-describedby`, `role`)
- Is focus management handled properly? (focus traps in modals, focus restoration)
- Are form labels associated correctly? (`<label for="id">` or wrap input)

### 6. Error Handling

- Are loading states shown? (spinners, skeletons, or messages)
- Are error states handled gracefully? (user-friendly messages, retry options)
- Are network failures caught and displayed?
- Are form validation errors clear? (inline, near the field)
- Are error boundaries in place? (React: `ErrorBoundary` components)

### 7. Security

- Is user input sanitized before rendering? (use proper escaping or sanitization libraries)
- Are XSS vulnerabilities prevented? (no unescaped user content in HTML)
- Are secrets kept out of frontend code? (no API keys, credentials in bundle)
- Is CSRF protection in place for forms? (token validation on backend)

### 8. Code Quality

- Are components single-responsibility? (one purpose per component)
- Are magic numbers/strings extracted to constants?
- Is complex logic extracted to custom hooks or utilities?
- Is code DRY? (avoid duplication, but don't over-abstract)
- Are naming conventions consistent? (PascalCase for components, camelCase for variables)

### 9. Testing

- Are critical user flows tested? (login, checkout, form submission)
- Are edge cases covered? (empty states, error states, loading states)
- Are tests independent and repeatable? (no shared state between tests)
- Are mocks used appropriately? (mock API calls, not implementation details)
- Is test coverage reasonable? (aim for >80% on critical paths)

## Output Format

```markdown
## Frontend Review: [Component/Feature Name]

### Critical Issues 🔴
- **[Issue Category]**: [Description]
  - **Location**: `path/to/file.tsx:42`
  - **Problem**: [What's wrong]
  - **Fix**: [Suggested solution]
  - **Impact**: [Why this matters]

### Warnings 🟡
- **[Issue Category]**: [Description]
  - **Location**: `path/to/file.tsx:15`
  - **Suggestion**: [How to improve]

### Accessibility Issues ♿
- **[WCAG Criterion]**: [Description]
  - **Location**: `path/to/file.tsx:28`
  - **Fix**: [How to make accessible]

### Performance Concerns ⚡
- **[Issue]**: [Description]
  - **Impact**: [Performance cost]
  - **Optimization**: [How to improve]

### Best Practices 🟢
- [Suggestion 1]: [Nice-to-have improvement]

### Questions ❓
- [Question 1]: [Clarification needed]

### Verified ✅
- [Aspect 1]: [What's done well]
```

## Important

- **Focus on user impact**: Performance, accessibility, security issues first
- **Don't nitpick style** unless it affects readability
- **Verify issues** before reporting (check docs, test code if unsure)
- **Prioritize**: Critical issues > Warnings > Nice-to-haves
- **Consider mobile**: Test responsive design, touch targets
- **Check console**: Look for errors/warnings in browser dev tools

## Common Issues to Watch For

### React
- Missing dependency arrays in `useEffect`
- Using array indexes as keys
- Prop drilling beyond 2 levels (consider context)
- Not cleaning up subscriptions/timers
- Mutating state directly (use spread operators or immutable patterns)

### Angular
- Not unsubscribing from observables
- Nested subscribes (use RxJS operators instead)
- Overusing `ngOnChanges` (prefer input setters or `OnPush`)
- Not using `trackBy` in large lists
- Providing services at wrong level

### Both
- Missing error boundaries
- No loading/error states
- Hardcoded text (should be i18n-ready)
- Inline styles (prefer utility classes)
- Missing ARIA labels on custom controls
