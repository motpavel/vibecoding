# Repository instructions for Codex

## Frontend UI/UX quality gate

After any task that changes frontend UI, visible components, pages, routes, layouts, CSS, Tailwind classes, design tokens, forms, navigation, responsive behavior, mobile behavior, visual states, or accessibility behavior, invoke `$ui-ux-reviewer` before considering the task complete.

Use the skill in repair mode when the task includes implementation, not just review.

Required workflow for frontend tasks:

1. Implement the requested change.
2. Run the normal project checks that are already available, such as lint, typecheck, tests, or build.
3. Invoke `$ui-ux-reviewer`.
4. Check the affected UI on mobile-first viewports whenever the environment allows it.
5. Fix all P0 and P1 findings before finalizing the task.
6. Report remaining P2 and P3 issues with concrete follow-up recommendations.

P0 and P1 findings are blocking. Do not mark frontend work as done while they remain unresolved, unless the user explicitly accepts the risk.

For mobile-heavy screens, prioritize these viewports:

- `320 x 568`
- `360 x 800`
- `390 x 844`
- `412 x 915`
- `768 x 1024`
- desktop width if relevant

When rendered review is blocked by missing environment variables, unavailable backend, authentication, seed data, or build failure, clearly say that the result is `static-only` and still provide a static UI/UX review from the changed code.

Do not add new UI libraries, icon packs, CSS frameworks, accessibility tools, or testing dependencies just for UI review unless the user explicitly asks for that.
