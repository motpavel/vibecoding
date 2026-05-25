---
name: ui-ux-reviewer
description: Review frontend UI/UX after implementation: mobile/responsive layout, typography, buttons, colors, spacing, accessibility, visual states, forms, navigation, and design-system consistency. Trigger for UI, UX, frontend, CSS, Tailwind, responsive, mobile, design review, интерфейс, мобильная версия, верстка. Do not use for backend-only changes.
---

# UI/UX Reviewer

## Purpose

Use this skill as a strict post-implementation UI/UX quality gate for frontend work. The goal is to catch visual, responsive, accessibility, and interaction defects before the task is considered done.

This is not a generic “make it pretty” pass. Review the rendered interface, compare it against the project’s design system and modern usability standards, then produce specific findings and fixes.

## When to use

Use this skill when a task changes any of the following:

- Pages, routes, layouts, modals, drawers, navigation, forms, tables, cards, dashboards, landing pages, onboarding, checkout, settings, profile, auth, search, filters, empty states, error states, loading states, or mobile views.
- CSS, Tailwind classes, CSS modules, styled components, theme tokens, design tokens, component variants, typography, color palettes, spacing, breakpoints, animations, or icons.
- Any frontend component that users can see, tap, click, type into, read, or navigate with a keyboard.

Do not use this skill for backend-only changes, database changes, CLI tools, pure refactors with no visual output, or tests that do not affect the user interface.

## Review posture

Be strict. Do not assume the UI is acceptable because the feature works.

Challenge weak implementation choices:

- “It compiles” is not enough.
- “It looks okay on desktop” is not enough.
- “The component exists” is not enough.
- “Mobile is responsive because it wraps” is not enough.
- “Color is from the design” is not enough if contrast, hierarchy, or state clarity is poor.

Prefer evidence over taste. When reporting an issue, tie it to a viewport, component, CSS rule, screenshot, DOM measurement, accessibility rule, or user task.

## Operating modes

### Report-only mode

Use report-only mode when the user asks for review, audit, QA, critique, PR comments, or “what should be fixed”. Do not modify files unless explicitly asked.

### Repair mode

Use repair mode when this skill is invoked as part of completing a frontend implementation, or when the user asks to fix UI/UX issues. In repair mode:

1. Audit first.
2. Fix P0 and P1 issues before finalizing the task when possible.
3. Fix low-risk P2 issues if they are clearly local and do not change product behavior.
4. Do not make broad redesigns without a clear design-system basis.
5. Report what was fixed and what remains.

## First steps

Before reviewing, understand the project context.

1. Inspect changed files:
   - `git diff --name-only`
   - `git diff --stat`
   - `git diff` for relevant frontend files
2. Identify the frontend stack:
   - `package.json`
   - app/router structure
   - framework config
   - styling setup
   - Storybook, Playwright, Cypress, Vitest, Jest, or other test tools
3. Find design-system sources:
   - `src/components`, `components/ui`, `app/components`, `shared/ui`
   - `tailwind.config.*`, theme files, token files, CSS variables
   - Storybook stories
   - existing pages with similar UI patterns
   - `AGENTS.md`, README, design docs, or contribution docs
4. Determine the user flows affected by the change.
5. Start the app using existing project scripts only. Prefer the repository’s documented commands.

Do not add new UI libraries, icon packs, animation libraries, or testing dependencies just to complete the review unless the user explicitly asked for that.

## Required evidence

A visual review is not complete if you only read the source code.

Use the best available evidence, in this order:

1. Render the affected UI locally and inspect it in a browser.
2. Capture screenshots for key viewports.
3. Inspect DOM metrics for tap targets, overflow, font sizes, and spacing.
4. Run existing accessibility, lint, typecheck, visual, or E2E checks if available.
5. If rendering is blocked, perform a static review and clearly label the result as “static-only”.

If the app cannot be run because of missing environment variables, auth, backend, seed data, or build failure, say exactly what blocked the rendered review and still provide a static UI/UX review from the code.

## Viewport matrix

At minimum, test changed UI on these viewports when the environment allows it:

- `320 x 568`: narrow mobile, catches overflow and cramped controls.
- `360 x 800`: common Android-sized mobile.
- `390 x 844`: modern iPhone-sized mobile.
- `412 x 915`: large mobile.
- `768 x 1024`: tablet.
- `1280 x 720` or `1440 x 900`: desktop.

For mobile-heavy features, prioritize the mobile viewports first. Many UI defects are invisible on desktop.

## Quality gates

### 1. Mobile and responsive layout

Check:

- No unintended horizontal scroll at any tested mobile width.
- Content does not touch screen edges unless intentionally full-bleed.
- Cards, tables, forms, and modals adapt cleanly on narrow screens.
- Sticky headers, sticky footers, cookie banners, chat widgets, and bottom CTAs do not cover important content or focused controls.
- Mobile menus, drawers, popovers, and modals fit the viewport and can be closed easily.
- Important actions remain reachable by thumb on mobile.
- Long text, translations, email addresses, IDs, prices, and error messages wrap safely.
- Images and media keep aspect ratio and do not crop meaningful content unexpectedly.
- `100vh` patterns do not create mobile browser chrome bugs; prefer safer dynamic viewport units where the project supports them.
- Safe-area insets are considered for bottom navigation, floating buttons, and full-screen mobile layouts.

Fail the review for P1 if mobile has overlapping content, clipped controls, impossible scrolling, broken modals, or hidden primary actions.

### 2. Typography and readability

Default expectations unless the project design system says otherwise:

- Body text is readable on mobile. Avoid tiny body copy; use at least `16px` for normal reading text in web UIs.
- Form inputs, selects, and textareas use readable font sizes and do not feel cramped.
- Secondary text can be smaller, but not so small that labels, hints, or prices become hard to read.
- Line-height supports scanning: body copy usually needs more breathing room than headings.
- Headings create a clear hierarchy. Do not use multiple competing “largest” styles on the same screen.
- Text blocks have reasonable width on desktop and do not become long unreadable lines.
- Text does not rely on image rendering unless essential.
- Font weight, size, and color are consistent with nearby existing components.

Flag inconsistent typography when it looks assembled from random classes rather than a system.

### 3. Buttons, tap targets, and interactive controls

Default expectations:

- Mobile touch targets should normally be at least `44 x 44 CSS px`; `48 x 48` is the preferred quality target for touch-heavy interfaces.
- The absolute lower bound for pointer targets is `24 x 24 CSS px`, and that is only acceptable for low-risk or constrained cases with enough spacing.
- Icon-only buttons must have a larger invisible hit area if the visual icon is small.
- Adjacent controls need enough spacing to prevent accidental taps.
- Primary, secondary, destructive, ghost, link, and icon buttons must be visually distinct.
- Button labels must describe the action, not generic words like “OK” when context is weak.
- Loading and disabled states must be visually and semantically clear.
- Destructive actions need confirmation or an undo path when the consequence is significant.
- Buttons must have visible focus states and accessible names.

Fail the review for P1 if primary mobile actions are too small, too close together, missing labels, hard to see, or easy to mistap.

### 4. Color, contrast, and visual states

Check:

- Normal text contrast meets at least `4.5:1` against its background.
- Large text contrast meets at least `3:1`.
- Meaningful non-text UI cues, such as input borders, focus rings, icons, chart marks, and control boundaries, meet at least `3:1` against adjacent colors.
- Color is not the only way to communicate error, success, warning, required, selected, or disabled states.
- Placeholder text is not used as the only label.
- Disabled controls are distinguishable but not confused with active controls.
- Dark mode is checked if the app supports it.
- Brand colors are used through tokens or theme variables where available, not scattered hard-coded values.

Fail the review for P1 if text, icons, form borders, selected states, or primary actions are too low-contrast to understand.

### 5. Spacing, alignment, and visual hierarchy

Check:

- Spacing follows the project’s grid or token scale. Random one-off values need justification.
- Related items are grouped visually; unrelated items are separated.
- Cards, buttons, fields, and list rows align consistently.
- Primary action is obvious. Secondary actions do not compete with it.
- Empty space supports comprehension rather than looking accidental.
- The page has a clear reading order from top to bottom.
- Dense screens still have scannable groups, headings, and dividers.
- There are no awkward widows, clipped labels, crushed icons, or misaligned baselines.

Flag visual noise: too many borders, shadows, colors, font sizes, button variants, or competing CTAs.

### 6. Forms and data entry

Check:

- Every input has a persistent label or an accessible label.
- Required fields are clear before submission.
- Error messages appear near the relevant field and explain how to fix the error.
- Validation does not destroy user input.
- Inputs have appropriate `type`, `inputmode`, `autocomplete`, and keyboard behavior.
- Mobile keyboards do not cover the field being edited or the submit action.
- Date, phone, money, password, file, and search inputs use suitable patterns.
- Success, error, saving, loading, and disabled states are handled.
- Long forms are grouped and can be scanned.

Fail the review for P1 if users can submit invalid data without clear recovery, cannot tell what went wrong, or cannot comfortably complete the form on mobile.

### 7. Navigation and user flow

Check:

- Users always know where they are and what to do next.
- Back, close, cancel, save, and destructive actions are predictable.
- Navigation is not hidden behind hover-only behavior.
- Mobile navigation is easy to open, use, and dismiss.
- Active states are visible.
- Breadcrumbs, tabs, steppers, pagination, and filters match existing patterns.
- Empty states and error states give a useful next action.
- Loading states avoid layout jumps and do not leave users guessing.

Use Nielsen-style heuristics: visibility of system status, match with user expectations, user control, consistency, error prevention, recognition over recall, and minimalist design.

### 8. Accessibility and keyboard behavior

Check:

- Semantic HTML is used before ARIA.
- ARIA is valid and not used to fake broken semantics.
- Interactive elements are keyboard reachable in a logical order.
- Focus is visible and not obscured by sticky UI.
- Modals trap focus while open and return focus when closed.
- Popovers, menus, and dialogs can be dismissed by keyboard.
- Headings are ordered logically.
- Images have useful alt text or are marked decorative.
- Status changes are announced when needed.
- Animations respect reduced-motion preferences.
- No keyboard traps.

Fail the review for P0 if a keyboard user cannot complete a core flow.

### 9. Component and design-system consistency

Check:

- Existing shared components are reused instead of creating lookalike one-offs.
- New variants belong in the component system rather than inside a page file when reused.
- CSS variables, Tailwind theme tokens, or design tokens are used consistently.
- Components support responsive states, disabled states, loading states, error states, focus states, and empty content.
- Fix root components when multiple screens share the same defect.
- Avoid page-specific hacks unless the problem is truly local.

Flag “almost the same” components as design debt.

### 10. Motion, polish, and perceived quality

Check:

- Animations are short, purposeful, and not required for understanding.
- Hover, active, pressed, selected, focus, loading, empty, success, warning, and error states feel intentional.
- Skeletons or spinners do not shift layout unexpectedly.
- Tooltips are not required to complete mobile tasks.
- Icons are aligned, sized consistently, and paired with text when meaning could be unclear.
- Number, date, currency, and plural formatting are user-friendly.
- Content does not flash or jump during loading.

Do not over-polish at the expense of clarity.

## Useful inspection snippets

Use these snippets through Playwright, browser devtools, or an equivalent runtime inspection method when possible.

### Find horizontal overflow

```js
(() => {
  const viewportWidth = document.documentElement.clientWidth;
  return [...document.querySelectorAll('body *')]
    .map((el) => {
      const r = el.getBoundingClientRect();
      return {
        tag: el.tagName.toLowerCase(),
        className: String(el.className || ''),
        id: el.id || '',
        left: Math.round(r.left),
        right: Math.round(r.right),
        width: Math.round(r.width),
        text: (el.innerText || el.getAttribute('aria-label') || '').trim().slice(0, 80)
      };
    })
    .filter((x) => x.width > 0 && (x.left < -1 || x.right > viewportWidth + 1))
    .slice(0, 50);
})();
```

### Find small interactive targets

```js
(() => {
  const selector = [
    'a[href]',
    'button',
    'input',
    'select',
    'textarea',
    'summary',
    '[role="button"]',
    '[role="link"]',
    '[role="menuitem"]',
    '[tabindex]:not([tabindex="-1"])'
  ].join(',');

  return [...document.querySelectorAll(selector)]
    .filter((el) => {
      const style = getComputedStyle(el);
      const r = el.getBoundingClientRect();
      return style.visibility !== 'hidden' && style.display !== 'none' && !el.disabled && r.width > 0 && r.height > 0;
    })
    .map((el) => {
      const r = el.getBoundingClientRect();
      return {
        tag: el.tagName.toLowerCase(),
        role: el.getAttribute('role') || '',
        label: (el.innerText || el.getAttribute('aria-label') || el.getAttribute('title') || el.getAttribute('name') || '').trim().slice(0, 80),
        width: Math.round(r.width),
        height: Math.round(r.height),
        className: String(el.className || '').slice(0, 120)
      };
    })
    .filter((x) => x.width < 44 || x.height < 44)
    .slice(0, 100);
})();
```

### Inspect font sizes

```js
(() => {
  return [...document.querySelectorAll('h1,h2,h3,h4,p,span,label,button,input,textarea,a,li')]
    .map((el) => {
      const style = getComputedStyle(el);
      const r = el.getBoundingClientRect();
      return {
        tag: el.tagName.toLowerCase(),
        text: (el.innerText || el.getAttribute('placeholder') || el.getAttribute('aria-label') || '').trim().slice(0, 80),
        fontSize: style.fontSize,
        lineHeight: style.lineHeight,
        fontWeight: style.fontWeight,
        width: Math.round(r.width),
        height: Math.round(r.height)
      };
    })
    .filter((x) => x.text && x.width > 0 && x.height > 0)
    .slice(0, 150);
})();
```

## Existing automated checks

Use existing tools when present, but do not treat them as a substitute for visual review.

Prefer existing project commands from `package.json`, for example:

- lint
- typecheck
- test
- build
- storybook
- playwright
- cypress
- lighthouse
- axe

If Playwright is already installed, prefer it for viewport screenshots and DOM measurements. If `@axe-core/playwright`, axe, or Lighthouse is already configured, run the relevant checks and include the results.

Do not install Playwright, Lighthouse, axe, or other dependencies just for this skill unless the user explicitly asked to add review tooling.

## Severity model

Use this severity model in reports.

### P0, blocker

Use P0 when the UI prevents completion of a core task or creates a serious accessibility failure.

Examples:

- Core action cannot be reached or activated.
- Modal traps the user.
- Keyboard user cannot complete the flow.
- Important content is invisible or covered.
- Form cannot be submitted or corrected.
- Navigation is broken.

### P1, must fix before merge

Use P1 for serious usability, mobile, accessibility, or visual regressions.

Examples:

- Mobile layout overlaps or horizontally scrolls unintentionally.
- Primary action is too small, hidden, or low-contrast.
- Text contrast fails for meaningful content.
- Form labels or errors are missing.
- Tap targets are too small or too close on mobile.
- Focus indicator is missing for interactive controls.
- Existing design-system pattern was bypassed.

### P2, should fix

Use P2 for quality problems that hurt polish, consistency, or comprehension but do not block the user.

Examples:

- Inconsistent spacing.
- Weak hierarchy.
- Slightly awkward wrapping.
- Inconsistent button variant.
- Missing empty or loading state polish.
- Too many competing visual treatments.

### P3, polish

Use P3 for small improvements.

Examples:

- Microcopy cleanup.
- Icon alignment by a few pixels.
- Minor animation polish.
- Better skeleton sizing.

## Report format

Always produce a structured report. Use the user’s language; default to Russian if the conversation is in Russian.

```md
# UI/UX Review

## Verdict

- Status: Pass / Pass with P2/P3 notes / Blocked by P0/P1 / Static-only review
- Merge readiness: Yes / No
- Main risk: one short sentence

## Scope checked

- Changed files:
- Routes/components reviewed:
- Viewports checked:
- Commands/checks run:
- Evidence limitations:

## Findings

| Severity | Viewport | Component / file | Problem | Evidence | Required fix |
|---|---:|---|---|---|---|
| P1 | 390x844 | `ComponentName` | Button tap target is too small | 32x36px measured in rendered DOM | Increase hit area to at least 44x44px, preferably 48x48px, using padding or component size token |

## Fix plan

1. Fix root component or token first.
2. Fix local layout only where necessary.
3. Re-check mobile viewports.
4. Re-run relevant checks.

## Suggested patch notes

- File:
- Change:
- Why:
- Risk:

## Verification checklist

- [ ] No unintended horizontal overflow on mobile.
- [ ] Primary actions are visible and tappable.
- [ ] Text and UI contrast meet minimums.
- [ ] Keyboard focus is visible and logical.
- [ ] Forms have labels, errors, and recovery paths.
- [ ] Loading, empty, error, disabled, hover, active, and focus states are covered where relevant.
- [ ] Design-system tokens/components are used instead of one-off styles.
```

Do not output vague comments like “improve spacing”, “make it modern”, or “looks good”. Every finding must name the concrete UI problem and the exact expected correction.

## Repair rules

When applying fixes:

1. Prefer design-system components and tokens over custom CSS.
2. Fix the smallest surface area that solves the issue.
3. If the same issue appears in multiple places, fix the shared component.
4. Keep behavior, API contracts, data flow, and business logic unchanged unless the UI bug requires a small behavior fix.
5. Avoid broad redesigns, new dependencies, or visual rewrites without user approval.
6. Preserve existing brand direction unless it conflicts with accessibility or usability.
7. After changes, re-run the relevant rendered checks or explain why they could not be run.

## Definition of done

The review is done only when:

- Affected UI has been checked against the viewport matrix or limitations are clearly stated.
- P0 and P1 issues are fixed or explicitly reported as blocking.
- P2 and P3 issues are listed with actionable fixes.
- Accessibility, responsive behavior, typography, colors, spacing, controls, states, and design-system consistency have all been considered.
- The final answer includes evidence, not just opinions.
