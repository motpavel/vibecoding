---
name: ui-ux-reviewer
description: Strict post-implementation UI/UX reviewer for frontend changes. Use after UI, UX, CSS, Tailwind, responsive, mobile, forms, navigation, components, colors, typography, accessibility, or design-system changes. Do not use for backend-only work.
---

# UI/UX Reviewer

## Mission

Use this skill as a strict quality gate after frontend implementation.

The goal is not to make the interface subjectively prettier. The goal is to catch defects users will actually feel: broken mobile layout, tiny tap targets, unreadable text, poor contrast, random spacing, unclear states, inconsistent components, inaccessible controls, weak hierarchy, and design-system drift.

Rendered UI is the source of truth. Reading code alone is not enough when rendering is possible.

## When to use

Use this skill after changes to:

- pages, routes, layouts, cards, tables, dashboards, landing pages, auth, profile, settings, search, filters, lists, modals, drawers, popovers, tabs, menus, forms, empty states, error states, loading states, and mobile views
- CSS, Tailwind classes, CSS modules, global styles, theme tokens, design tokens, color palettes, typography, spacing, breakpoints, animations, icons, and shared UI components
- anything users can see, tap, click, type into, read, navigate, confirm, cancel, save, or delete

Do not use this skill for backend-only changes, database changes, CLI tools, non-visual refactors, or tests that do not affect user-visible UI.

## Review posture

Be strict and evidence-based.

Reject these weak assumptions:

- It compiles, so it is done.
- It works on desktop, so mobile is fine.
- Responsive means elements wrap somehow.
- A visible icon means a usable button.
- Placeholder text can replace labels.
- Disabled, loading, empty, error, hover, focus, active, and selected states can be skipped.
- Brand color automatically has good contrast.
- One-off CSS is acceptable because it is faster.

Every serious finding must name the concrete problem, where it appears, evidence, and the required fix.

## Operating modes

### Audit mode

Use when the user asks for review, critique, QA, PR comments, or findings only.

Output a structured report. Do not edit files unless asked.

### Repair mode

Use after frontend implementation or when the user asks to fix UI/UX defects.

1. Audit first.
2. Fix all P0 and P1 issues before finishing when possible.
3. Fix local low-risk P2 issues if they are obvious and do not change product behavior.
4. Avoid broad redesign unless the current UI is structurally broken or the user asked for redesign.
5. Re-check the changed UI after fixes.
6. Report what was fixed and what remains.

## First steps

Before judging the UI:

1. Inspect changed files with `git diff --name-only`, `git diff --stat`, and relevant `git diff` sections.
2. Identify the frontend stack from `package.json`, framework config, app/router structure, and styling setup.
3. Find the design system: `components`, `components/ui`, `shared/ui`, theme files, CSS variables, Tailwind config, tokens, Storybook stories, and similar existing screens.
4. Identify the affected user flow and the primary action.
5. Run the app using existing project scripts only.
6. Use existing checks only: lint, typecheck, test, build, Playwright, Cypress, Storybook, Lighthouse, or axe if already configured.

Do not add UI libraries, icon packs, CSS frameworks, testing tools, or accessibility dependencies just for review unless the user explicitly asks.

## Required evidence

Use the best available evidence:

1. Render the affected UI locally.
2. Check the relevant routes or components in a browser.
3. Inspect key mobile and desktop viewports.
4. Capture screenshots if the environment supports it.
5. Measure tap targets, overflow, font sizes, spacing, and focus behavior when possible.
6. Run existing automated checks when relevant.
7. If rendering is blocked, mark the result as `static-only` and explain the blocker.

Rendering blockers can include missing environment variables, unavailable backend, auth, seed data, unclear route, broken build, or missing scripts. Still provide a static UI/UX review from the code.

## Viewport matrix

Check changed UI on these viewports when possible:

- `320 x 568` narrow mobile
- `360 x 800` common Android mobile
- `390 x 844` common iPhone mobile
- `412 x 915` large mobile
- `768 x 1024` tablet
- `1280 x 720` or `1440 x 900` desktop

For mobile-heavy features, start with mobile. If it fails at 320-390px, it is not ready.

## Quality gates

### 1. User task and hierarchy

Check that the screen has one obvious primary task, a clear primary action, predictable secondary actions, a readable order, visible system status, and no competing CTAs. Flag as P1 if the main action is hidden, ambiguous, weak, or hard to reach on mobile.

### 2. Mobile and responsive layout

Check for unintended horizontal scroll, clipped content, edge-cramped layouts, broken cards, broken tables, broken grids, modals that do not fit, sticky UI covering content, unsafe `100vh`, missing safe-area handling, and long text that breaks layout.

P1 examples: horizontal overflow, clipped primary button, modal that cannot scroll or close, bottom CTA covering content, layout that only works on desktop.

### 3. Typography and readability

Normal body text should usually be at least `16px` on mobile unless the design system says otherwise. Inputs must be readable. Line-height must support scanning. Headings must create hierarchy. Useful secondary text must not look disabled. Long text blocks need reasonable desktop width.

P1 examples: unreadable labels, prices, errors, button text, or form content.

### 4. Buttons and tap targets

Mobile touch targets should normally be at least `44 x 44 CSS px`. `48 x 48` is preferred for touch-heavy screens. `24 x 24` is only an absolute minimum for constrained, low-risk cases with enough spacing. Icon-only buttons need accessible names and a larger hit area. Adjacent controls need spacing.

P1 examples: primary action under 44px high, adjacent icon buttons too close, icon button without accessible name, destructive action styled like a normal action, loading state allows double submit.

### 5. Color, contrast, and visual states

Normal text contrast should meet `4.5:1`. Large text should meet `3:1`. Important non-text cues, borders, icons, focus rings, selected states, and input outlines should meet `3:1`. Do not communicate status by color alone. Placeholder text must not be the only label. Check dark mode if supported.

P1 examples: low-contrast primary button text, low-contrast errors, invisible focus ring, selected state shown only by color.

### 6. Spacing, alignment, and density

Spacing should follow the project scale or tokens. Related items should be grouped, unrelated items separated. Cards, fields, icons, buttons, badges, and list rows should align. Dense screens still need headings and groups. Avoid random one-off spacing and visual noise.

### 7. Forms and data entry

Every input needs a visible or reliable accessible label. Required fields must be clear before submit. Errors must appear near the field and explain recovery. Validation must not erase input. Use appropriate `type`, `inputmode`, and `autocomplete`. Mobile keyboards must not block the active field or submit action.

P1 examples: missing labels, vague errors, disabled submit with no explanation, failed submit with no recovery path.

### 8. Navigation and flow

Users must know where they are, what changed, and what to do next. Back, close, cancel, save, next, skip, destructive actions, active tabs, selected filters, pagination, empty states, and loading states must be predictable. Avoid hover-only navigation.

### 9. Accessibility and keyboard behavior

Use semantic HTML before ARIA. Interactive elements must be keyboard reachable in a logical order. Focus must be visible. Dialogs should trap focus while open and restore focus when closed. Menus and popovers must be dismissible by keyboard. Images need useful alt text or decorative marking. Respect reduced motion.

P0 examples: keyboard user cannot complete a core flow, modal traps user permanently, critical control is not reachable.

P1 examples: missing focus indicator, clickable div used instead of button, input has no accessible name, dialog does not restore focus.

### 10. Component and design-system consistency

Reuse existing shared components instead of creating lookalike one-offs. Put reusable variants into the component system. Use tokens, CSS variables, or Tailwind theme values consistently. If the same defect appears in multiple places, fix the shared component.

Flag almost-the-same components as design debt.

### 11. Content and localization

Labels and headings must be specific. Errors must say what happened and how to recover. Dates, times, numbers, currencies, percentages, and plural forms should match locale. Long names, emails, URLs, IDs, and translations must not break layout.

### 12. Motion and polish

Motion should be short, useful, and not required to understand the UI. Hover, active, pressed, selected, focus, disabled, loading, empty, success, warning, and error states should feel intentional. Skeletons and spinners should not cause major layout shifts. Tooltips must not be required for mobile task completion.

## Severity model

### P0, blocker

Core task cannot be completed, critical content is inaccessible, form cannot be submitted or corrected, modal traps the user, keyboard user cannot complete the flow, or navigation is broken.

### P1, must fix before completion

Serious mobile, usability, accessibility, or visual regression: overlap, hidden action, low contrast, tiny tap target, missing labels, missing focus, broken modal, unclear destructive action, or bypassed design-system pattern.

### P2, should fix

Quality problem that hurts polish, consistency, trust, or comprehension but does not block: weak hierarchy, inconsistent spacing, awkward wrapping, inconsistent variant, missing empty/loading polish, too many competing visual treatments.

### P3, polish

Small improvement: microcopy cleanup, icon alignment, minor animation polish, better skeleton sizing.

## Report format

Always produce a structured report in the user language.

```md
# UI/UX Review

## Verdict

- Status: Pass / Pass with P2-P3 notes / Blocked by P0-P1 / Static-only review
- Merge readiness: Yes / No
- Main risk:

## Scope checked

- Changed files:
- Routes/components reviewed:
- Viewports checked:
- Commands/checks run:
- Evidence limitations:

## Findings

| Severity | Viewport | Component / file | Problem | Evidence | Required fix |
|---|---:|---|---|---|---|
| P1 | 390x844 | `ComponentName` | Button tap target is too small | 32x36px measured in rendered DOM | Increase hit area to at least 44x44px, preferably 48x48px |

## Fix plan

1. Fix root component or token first.
2. Fix local layout only where necessary.
3. Re-check mobile viewports.
4. Re-run relevant checks.

## Verification checklist

- [ ] No unintended horizontal overflow on mobile.
- [ ] Primary actions are visible and tappable.
- [ ] Text and UI contrast meet minimums.
- [ ] Keyboard focus is visible and logical.
- [ ] Forms have labels, errors, and recovery paths.
- [ ] Loading, empty, error, disabled, hover, active, selected, and focus states are covered where relevant.
- [ ] Design-system tokens/components are used instead of one-off styles.
```

Do not write vague comments like `improve spacing`, `make it modern`, or `looks good`. Name the exact defect and exact correction.

## Repair rules

When applying fixes:

1. Prefer shared components, theme tokens, and design-system utilities over custom CSS.
2. Fix the root component when the problem is systemic.
3. Fix the smallest surface area that solves the issue.
4. Keep product behavior, API contracts, data flow, and business logic unchanged unless the UI defect requires a small behavior fix.
5. Do not add new dependencies without explicit user approval.
6. Preserve existing brand direction unless it conflicts with usability or accessibility.
7. Avoid broad redesigns unless the current UI is structurally broken.
8. Re-check the affected viewports after changes or state why it was not possible.

## Final answer requirements

When finishing a frontend task after using this skill, include:

1. What UI was reviewed.
2. Which viewports were checked.
3. P0/P1 findings fixed, if any.
4. Remaining P2/P3 notes, if any.
5. Commands/checks run.
6. Limitations, such as static-only review.

## Definition of done

The review is complete only when:

- Affected UI was checked against the viewport matrix or limitations are clearly stated.
- P0 and P1 issues are fixed or explicitly reported as blocking.
- P2 and P3 issues are listed with actionable fixes.
- Accessibility, responsive behavior, typography, colors, spacing, controls, states, content, and design-system consistency have all been considered.
- The final report contains evidence, not just opinions.
