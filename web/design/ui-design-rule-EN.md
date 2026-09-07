# UI Design Rules

UI design and verification rules for designers, developers, and AI agents working on web projects.

## 1. Scope and Priority

- **Scope:** Changes to web pages, reusable components, design tokens, colors, and themes.
- **Required:** Rules that implementation and review must satisfy.
- **Recommended:** Guidelines that may be adapted to the purpose of the screen and the existing design system. Record the reason for choosing a different approach.
- Review explicit project requirements, the design system, components, and token naming conventions first. Use this document as the default where existing rules do not specify behavior.
- Check accessibility when reusing existing styles. Do not preserve a contrast failure solely because it uses a brand color or an existing implementation.
- Contrast requirements follow the relevant WCAG 2.2 Level AA success criteria. This document alone does not establish full WCAG conformance.

## 2. Before Starting — Required

1. Read the project's `AGENTS.md`, UI documentation, shared styles, token definitions, and relevant components.
2. Identify the existing palette, semantic tokens, theme implementation, and supported viewport sizes.
3. Identify the states needed for the task, such as default, hover, focus-visible, pressed, selected, disabled, loading, and error.
4. Reuse components and tokens that already serve the same purpose. Do not create duplicate tokens under different names.
5. If the project has no token system, choose and document one shared location for definitions. Do not arbitrarily impose a library or color family.

## 3. Color Palettes

### Required

- Manage colors through palettes with consistent scales and defined purposes.
- Use existing palettes first. Extend them only when they cannot provide the required lightness or role.
- For new palettes, provide the needed range from light to dark. Keep scale names and ordering consistent.
- Check actual foreground and background combinations. Do not infer readability from a color's scale number.
- Do not require every step in a palette to be created or used.

### Recommended

- Distinguish neutral colors for backgrounds and surfaces from brand and status colors.
- If no convention exists, consider an ordered scale such as `50`, `100`, …, `900`, `950`. This is an optional naming example.

## 4. Semantic Color Tokens — Required

- Separate raw color values from their roles in the UI.
- Components must reference semantic tokens appropriate to their purpose. Do not repeatedly hardcode raw UI colors such as HEX, RGB, or HSL values in individual components.
- Manage raw values in shared palette or token definitions, and map semantic tokens to those values.
- Preserve the existing mechanism, whether CSS variables, theme objects, or utility classes. Do not introduce a parallel system for the same meaning.
- Validate foreground tokens for text and icons against their intended background tokens. Do not assume text on a brand color should always be white.
- Define dedicated palette rules for domains with different roles, such as chart series. Document the scope and reason for exceptions involving external content or assets.

The following names illustrate roles. They do not require renaming existing project tokens.

| Role | Example token | Purpose |
| --- | --- | --- |
| Page background | `bg.canvas` | The screen's base background |
| Surface background | `bg.surface` | Cards, panels, and popups |
| Primary text | `text.primary` | Essential information, including headings and body text |
| Secondary text | `text.secondary` | Descriptions and supplementary information; contrast requirements still apply |
| Brand | `brand.primary` | Brand identity color |
| Primary action | `action.primary.bg` / `action.primary.fg` | Primary button background and foreground |
| Error | `status.error` | Error messages and status indicators |
| Dividers and boundaries | `border.default` | Lines separating elements |
| Keyboard focus | `focus.ring` | The current keyboard interaction target |

## 5. Color Balance and Visual Hierarchy — Recommended

Use **60:30:10** as a starting point for color balance.

| Proportion | Role | Application |
| --- | --- | --- |
| About 60% | Dominant neutral | Large backgrounds and base surfaces |
| About 30% | Secondary color | Section separation and supporting visual elements |
| About 10% | Accent color | Primary actions and important information |

- Treat these proportions as a visual guideline. Do not enforce exact ratios using pixel area or DOM element counts.
- Concentrate accents on high-priority information and actions. Avoid giving many elements equal emphasis at the expense of hierarchy.
- The meaning and readability of error, warning, and success states take priority over decorative color proportions.
- Adapt the proportions to the purpose of data visualizations, brand-focused screens, and other specialized layouts.

## 6. Contrast and State Communication — Required

### Contrast Thresholds

| Target | Minimum contrast | Colors to compare |
| --- | --- | --- |
| Normal text and images of text | **4.5:1** | Text against its actual background |
| Large text and corresponding images of text | **3:1** | Text against its actual background |
| Visual information needed to identify controls and states, and graphics needed to understand content | **3:1** | The identifying parts against adjacent colors |

Large text generally means **at least 18pt, or at least 14pt bold**, approximately 24px or 18.67px bold in CSS. Check the actual font and rendered size. If large-text eligibility is unclear, apply 4.5:1. Text thresholds and exceptions follow [W3C's Contrast (Minimum) guidance](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html).

The 3:1 non-text threshold applies to visual information needed to identify controls, states, or content, not to every decorative line. It does not require a 3:1 difference between default and hover colors. Compare the identifying elements with adjacent colors in each state. [W3C's Non-text Contrast guidance](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html)

### Verification Method

- Judge the calculated ratio against the threshold. Do not round 4.499:1 up to a passing 4.5:1.
- Check secondary text, placeholders, button labels, and text displayed on hover or focus against the applicable threshold.
- For transparency, gradients, or image backgrounds, check the lowest-contrast area in the actual composited result.
- Check implemented themes and relevant interaction states. A passing default-screen check does not establish that every state passes.
- Supplement automated checks with contrast calculations and visual review where automation cannot determine the result. Record unchecked items.

### Exceptions and State Communication

- Inactive controls, pure decoration, and logos have exceptions under the applicable success criteria. Record the target and justification when applying an exception. Supplementary information is not exempt simply because it uses a faint color.
- Do not communicate errors, success, or selection through color alone. Add text, icons, shapes, or clear state indicators.
- Keep keyboard focus visible. When removing the default outline, provide a distinguishable replacement.

## 7. Light and Dark Themes — Required Where Supported

- Keep semantic tokens stable and change their color mappings by theme.
- Manage mappings in the existing token or theme layer. Do not repeat raw theme-specific colors in components.
- Do not treat mechanical inversion of all colors as a completed theme implementation.
- Validate backgrounds, surfaces, text, borders, foregrounds on brand colors, status colors, and focus indicators in each theme.
- Check implemented component states, including hover, focus-visible, selected, disabled, loading, and error where applicable.
- Check that theme-sensitive assets, such as images, logos, and charts, remain identifiable in each theme.
- Follow the existing policy for saving theme preferences and respecting system settings. If a new implementation needs a policy and none exists, define and document the behavior.
- Do not add dark mode to a single-theme project without a requirement. Apply these rules within the supported theme scope.

## 8. Components and Layout

- **Required — Components:** Reuse shared buttons, inputs, cards, and other existing components first. Avoid introducing inconsistent styles for the same role.
- **Required — Typography and spacing:** Follow existing scales for font size, weight, line height, spacing, and border radius. If no system exists, define recurring values centrally.
- **Required — Responsive layouts:** Check information order, wrapping, overlap, clipping, and operability at the project's supported small and large viewport sizes.
- **Required — States and feedback:** Provide loading, empty, error, and completion states needed by the changed flow. Make errors clear about where they occurred and how to resolve them.
- **Required — Interaction:** Prefer native HTML elements with appropriate semantics, and verify that the relevant flow can be completed using the keyboard.
- **Recommended — Decoration and motion:** Use decoration and animation to support understanding. When adding effects, provide a usable presentation with reduced-motion settings.

## 9. Completion Checklist

Verify the items relevant to the change. Record why any item does not apply.

- [ ] Existing tokens and components were reviewed and reused.
- [ ] New raw colors are centrally defined; no duplicate tokens or unnecessary hardcoded values were introduced.
- [ ] Semantic token roles match their actual usage.
- [ ] Contrast was checked against the applicable thresholds for normal text, large text, and meaningful non-text elements.
- [ ] Supported themes and relevant states of changed components were checked.
- [ ] Errors, success, and selection are not communicated through color alone.
- [ ] Keyboard focus is visible and relevant interactions work with the keyboard.
- [ ] Reading and interaction work at the project's supported viewport sizes.
- [ ] Relevant project verification commands were run and their results checked. Obtain commands from the actual project configuration.
- [ ] Unchecked items, unmet requirements, and exceptions are stated in the completion report.

The completion report must identify **changed components and tokens; checked themes, states, and viewport sizes; contrast results; verification performed; and unchecked items**.

## 10. Referencing This File from AGENTS.md

If this file is in the project root, add the following instructions to the project's `AGENTS.md`. Adjust the path if the file is elsewhere. Reference only one language version to avoid loading duplicate instructions.

```md
## UI Rules

- Before creating or modifying UI, read and apply UI_DESIGN_RULES_ENG.md in the project root.
- Reuse the existing design system and tokens first.
- Verify the items in the UI_DESIGN_RULES_ENG.md completion checklist that apply to the change.
- Include verification results and unchecked items in the completion report.
```

Record project-specific token locations, supported themes, viewport sizes, and verification commands in the existing project documentation. Do not assume every tool loads this filename automatically; reference it from the instruction file used by the tool.
