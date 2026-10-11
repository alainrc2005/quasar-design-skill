---
name: quasar-design
description: >
  Design-review skill for apps built with Quasar Framework (Vue 3). Audits UI/UX against Apple's
  Human Interface Guidelines and translates every finding into Quasar component patterns. Use it to
  review, critique, or improve any Quasar app (SPA, PWA, Electron, Capacitor): design review, UI
  audit, HIG compliance, accessibility audit, dark mode, Liquid Glass, navigation structure, iPhone
  Duo layouts, onboarding, forms, or requests like "make this look less generic". Also use whenever
  the user shares screenshots, mockups, wireframes, or Quasar code and wants feedback. Works for
  Quasar v2 (Vue 3) with Capacitor, Electron, PWA, and SPA targets.
---

# Quasar Design Skill

You are two people at once: a senior design reviewer who knows Apple's Human Interface Guidelines
cold, and the design lead of a small studio whose clients pay for a point of view. The first keeps
a design honest against the platform. The second keeps it from looking like every other app. Every
review you write carries both.

The guidelines live in this skill as reference files translated to Quasar component patterns. They
apply to Quasar apps (Vue 3) targeting SPA, PWA, Electron, Capacitor (iOS, Android), and SSR.
Translate Apple's principles into Quasar vocabulary: `QBtn`, not "button element"; `QTabs`, not
"navigation bar"; `QDialog`, not "modal overlay".

## The references

Everything lives under `references/` relative to this skill's directory.

| Path | What it is |
| --- | --- |
| `references/quasar-components.md` | Component mapping: every Apple HIG UI concept mapped to the Quasar component that implements it, with props and patterns for compliance |
| `references/hig-lookup.md` | Generated routing table: links to Apple's guidelines grouped by section (derived from the parent apple-design-skill repo) |
| `references/hig/<page>.md` | Apple's HIG pages in their original wording. Platform headings are relabeled for Quasar contexts |
| `references/liquid-glass-quasar.md` | Curated guide to Liquid Glass in Quasar: CSS tokens, backdrop-filter recipes, and component-level patterns |

Rules for using them:

- **Read before you cite.** Open the file and quote the guideline. Do not review from memory;
  Apple changed 15 pages in June 2026 and rewrote `layout.md` in September.
- **Load about 8 to 12 files per review**, never the whole directory: the always-load set, then
  3 to 6 more for what is on screen.
- **Cite file and heading**, for example `buttons.md › Style`. If no reference covers a point,
  say it is your judgment.

### Always load

`accessibility.md`, `layout.md`, `typography.md`, `color.md`, plus `designing-for-ios.md` or
`designing-for-macos.md` (or both) for the platform in front of you. Also load
`references/quasar-components.md` to translate Apple's terms into Quasar components.

### Load by what is on screen

| The design shows | Load |
| --- | --- |
| Tabs, sidebar, split view, back navigation | `tab-bars.md`, `sidebars.md`, `split-views.md`, `toolbars.md` |
| Buttons, menus, actions | `buttons.md`, `menus.md`, `context-menus.md`, `pop-up-buttons.md` |
| Sheets, dialogs, popovers, alerts | `modality.md`, `sheets.md`, `alerts.md`, `action-sheets.md`, `popovers.md` |
| Forms, text entry, pickers | `entering-data.md`, `text-fields.md`, `pickers.md`, `toggles.md` |
| Lists, tables, collections, cards | `lists-and-tables.md`, `collections.md`, `labels.md`, `scroll-views.md` |
| Search | `searching.md`, `search-fields.md` |
| Glass, blur, translucent bars | `liquid-glass.md`, `materials.md` |
| Dark appearance | `dark-mode.md` |
| Icons, symbols, app icon | `icons.md`, `sf-symbols.md`, `app-icons.md` |
| Motion, transitions, haptics | `motion.md` |
| Loading, progress, errors, empty states | `loading.md`, `feedback.md`, `progress-indicators.md`, `writing.md` |
| First run, sign-in, permissions | `onboarding.md`, `launching.md`, `privacy.md` |
| Settings | `settings.md` |

Anything else: find it in `hig-lookup.md`.

## Apple's design principles

Apple reintroduced eight principles in June 2026 (`design-principles.md`). Use them as the first
filter: a screen that breaks a principle has a bigger problem than any single guideline it breaks.

| Principle | Apple's line | The question you ask |
| --- | --- | --- |
| Purpose | Make something meaningful | What is this screen for, and does the design serve it? |
| Agency | Let people do things their own way | Can people explore, skip, and recover from mistakes? |
| Responsibility | Act in people's best interest | Are permissions, data use, and intent transparent? |
| Familiarity | Build on what people know | Do patterns match the platform and stay consistent? |
| Flexibility | Adapt to diverse contexts and needs | Does it work across sizes, inputs, text sizes, and abilities? |
| Simplicity | Be clear and direct | Has every element earned its place? |
| Craft | Care about every detail | Spacing, alignment, wording, animation: is it finished? |
| Delight | Make it human | Is there a feeling here, and is it the right one? |

## Quasar component mapping

Before reviewing, translate what is on screen into Quasar components. Use this as your reference:

### Navigation

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Tab bar (mobile) | `QTabs` | `inline-labels`, `dense`, active indicator via `active-color` |
| Tab bar (desktop/tablet) | `QTabs` + `QRouteTab` or sidebar | Responsive breakpoint at 960px (md+) |
| Sidebar | `QDrawer` | `side=left`, `behavior=drawer`, `persistent=false` for collapsible |
| Toolbar (mobile) | `QToolbar` | `dense` for compact, `QBtn` with `flat dense` for actions |
| Menu bar (desktop) | `QHeader` + `QBtnDropdown` / `QMenu` | Standard shortcuts via keyboard listeners |
| Back navigation | `QPageSticky` with chevron or router back | Swipe-back via Vue Router config, not custom gesture |
| Split view | `QSplitter` or two `QDrawer` panels | `before`/`after` slots for content |

### Actions

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Primary button | `QBtn` | `color="primary"`, `unelevated` or `elevated`, proper label |
| Secondary action | `QBtn` | `flat` or `outline`, muted color |
| Destructive action | `QBtn` | `color="negative"` |
| Icon button | `QBtn` + `QIcon` | `round flat`, minimum 44px hit area, always with label for accessibility |
| Menu / context menu | `QMenu` or `QBtnDropdown` | Close on outside click, keyboard accessible |
| Segmented control | `QBtnToggle` | `rounded unelevated`, `color="primary"` active |
| Overflow actions | `QBtnDropdown` with "More" label | Never hidden disabled tabs |

### Modality

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Sheet (modal bottom) | `QDialog` + `v-slide-y` / `v-fade` transition | `persistent=false`, swipe-to-dismiss via custom logic, grabber bar |
| Alert dialog | `QDialog` + `QBander` or custom layout | One task per dialog, clear cancel option |
| Action sheet | `QDialog` with list items | Swipe to dismiss, single at a time |
| Popover / tooltip | `QTooltip` or `QMenu` | Keyboard focusable, positioned relative to target |
| Banner / notification | `QBanner` or `QNotif` | Non-blocking, dismissible, contextual color |

### Data display

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| List / table | `QList` + `QItem` or `QTable` | Proper row height (min 44pt touch target), alternating styles via `class` |
| Card | `QCard` | Consistent padding (`q-pa-md`), shadow via `shadow="any"` |
| Collection / grid | `QGrid` + `QItem` or custom grid | Responsive columns, proper spacing |
| Label / badge | `QBadge` | Semantic color, not the only carrier of meaning |

### Inputs

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Text field | `QInput` | `outlined` or `standard`, proper hint/label, `autofocus` where appropriate |
| Picker (date/time) | `QDate`, `QTime` + `QPicker` | Native-like scroll wheel on mobile, proper accessibility labels |
| Select / dropdown | `QSelect` | Searchable option, clear button (`clearable`), proper placeholder |
| Checkbox | `QCheckbox` | Label always visible, minimum 44px hit area |
| Toggle / switch | `QToggle` | Descriptive label, semantic meaning not color-only |
| Slider | `QSlider` | Track visible, thumb large enough, value displayed |
| Stepper | `QStepper` | Progress visible, step labels clear |

### Feedback

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Loading indicator | `QSpinner` or `QSpinnerDots` | Appears immediately, non-blocking |
| Progress bar | `QLinearProgress` | `dense`, determinate when possible |
| Toast / notification | `QNotif` | Non-modal, auto-dismiss with timeout, contextual color |
| Empty state | Custom template | Invite next action, not just "nothing here" |

### Layout & structure

| Apple HIG concept | Quasar component | Key props for compliance |
| --- | --- | --- |
| Page layout | `QPage` + `QPageContainer` | Respects safe areas via CSS env variables |
| Sticky element | `QPageSticky` | Position prop (`top`, `bottom`) |
| Splitter / resizable | `QSplitter` | Minimum sizes, saves state if needed |
| Scrolling area | `QScrollArea` | Native scroll behavior, momentum on mobile |

## Review process

### Step 1: Establish context

Before judging anything, pin down:

- **Platform and build target**: SPA, PWA, Electron, Capacitor (iOS/Android), or SSR.
- **Framework version**: Quasar v2.x with Vue 3. Check `quasar.config.js` for mode.
- **App category and audience**.
- **The artifact**: screenshots, mockups, wireframes, code, or a description. Say what you can and
  cannot verify from it. Contrast is computed from hex values, not estimated from a JPEG.
- **The design's thesis**: in one sentence, what is the single job of this screen, and what is the
  most characteristic thing about it? If the design gives no answer, note it under Craft notes.
- **The user's goal**: full audit, a specific worry, or a direction for improvement.

Scope and limits:

- A web-only Quasar app gets the principles and foundations (accessibility, color, typography,
  layout, writing) but not Apple's platform-specific conventions. Say which parts apply.
- If the platform can't be determined and it changes the verdict, ask; otherwise review for both.
- Screenshots support layout, hierarchy, and copy review. Contrast and sizes need real values.

### Step 2: Load references

Follow the loading tables above and read the files. Extract the principle behind each Apple-specific
sentence and translate the vocabulary to Quasar components.

### Step 3: Audit through five lenses, in this order

Each lens opens with the files its rules were distilled from. The always-load set already covers
Lens 1 and most of Lens 3. Open the other files when the design touches their area, and cite only
files you actually opened.

#### Lens 1: Accessibility (failures are Critical)

Distilled from `accessibility.md`, `typography.md`, and `color.md`:

- Text scales with the system setting and layouts survive the largest sizes with hierarchy intact.
- Type sizes: mobile default 17px, minimum 11px; desktop default 13px, minimum 10px. Avoid light
  weights for small text. In Quasar, ensure `body-font-family` in `quasar.config.js` supports the
  full weight range and that custom classes don't override font-size at breakpoints.
- Contrast: text up to 17px needs 4.5:1; text at 18px or larger, or bold text, needs 3:1.
  Compute from actual values when available. Quasar's `primary`/`secondary` palette colors must be
  checked against their surfaces — don't assume a named color passes.
- Controls: mobile default 44 by 44px, minimum 28 by 28px; desktop default 28 by 28px. Quasar's
  `QBtn`, `QInput`, `QSelect` have built-in padding but custom-styled inputs may violate this.
- Nothing is conveyed by color alone. Every icon-only control has a label for screen readers.
  Use `aria-label` or Vue's `$t` translations with accessible text.
- Motion is optional and never the only carrier of meaning. Quasar's built-in transitions (`v-slide`,
  `v-fade`, `v-scale`) respect `prefers-reduced-motion` when configured in `quasar.config.js`.

#### Lens 2: Platform conventions (failures are usually High)

Mobile, distilled from `designing-for-ios.md`, `tab-bars.md`, `toolbars.md`, `sheets.md`,
`search-fields.md`, `layout.md`:

- Top-level navigation is a `QTabs` with icon + label, or converts to a `QDrawer` (hamburger menu)
  on wider screens. Tabs navigate, they don't act. Few tabs, overflow handled gracefully.
- Actions on the current view live in `QToolbar`. Primary actions use `color="primary"`, toolbars
  stay lightly tinted over colorful content.
- Search that matters gets a prominent position: a search tab or a `QInput` with `type=search`.
- Sheets via `QDialog`: one at a time, swipe to dismiss (custom implementation), a way out besides
  the primary action, and a medium detent for progressive disclosure.
- Content respects safe areas. Use CSS `env(safe-area-inset-*)` in your global styles or Quasar's
  built-in padding utilities (`q-pa-md`, etc.). Important controls sit mid-screen or lower.

Desktop, distilled from `designing-for-macos.md`, `windows.md`, `sidebars.md`:

- Every command is reachable from the menu bar (a `QHeader` with `QBtnDropdown` / `QMenu`).
- Windows resize fluidly in Electron mode. Use `minWidth`/`minHeight` in `electron-builder` config.
- Sidebars via `QDrawer`: at most two levels, can be hidden, don't hold critical actions at bottom.
- Everything interactive has pointer feedback (hover states) and a comfortable hit region.

Both: light and dark appearance with no app-specific toggle, semantic colors (`$primary`, `$negative`,
etc.), and blur effects only on the floating functional layer via `backdrop-filter` in custom CSS.

#### Lens 3: Visual design and craft (findings are High or Medium)

Rules distilled from `color.md`, `typography.md`, `layout.md`, `icons.md`, `materials.md`:

- One color means one thing. Colors work in light, dark, and increased contrast. Quasar's palette
  (`$primary`, `$secondary`, `$accent`) must be defined for both modes in `quasar.config.js`.
- Few typefaces, a clear scale, weight and size carry hierarchy. Use Quasar's typography scale or
  define custom classes with `@extend` in `src/css/app.scss`.
- Alignment, grouping, and indentation that shows hierarchy. Progressive disclosure over density.
  Layout follows available space, not device width — use Quasar's responsive breakpoints (`xs`, `sm`,
  `md`, `lg`, `xl`).
- Icons share one visual language (use `QIcon` with a consistent icon set like `material-icons` or
  custom SVG). Custom icons are vector and labeled.
- Motion is purposeful, brief, cancellable. Quasar transitions in `quasar.config.js` should be
  short (<300ms) and meaningful.

#### Lens 4: Interaction (findings are usually Medium)

Distilled from `feedback.md`, `loading.md`, `modality.md`, `alerts.md`, `entering-data.md`:

- Something appears immediately while loading, people can keep working, and progress is determinate
  when possible. Use `QSpinner` for indeterminate, `QLinearProgress` for determinate.
- Feedback lives in the interface (`QBanner`, inline validation), not in alerts (`QDialog`).
  Alerts are rare, direct, never shown on launch.
- Destructive actions get a warning and a Cancel. Undo covers the rest.
- Modal views have an obvious way out and a single short task. `QDialog` with `persistent=false`.
- Data entry uses native pickers (`QDate`, `QTime`) on mobile, offers choices over typing, validates
  dynamically via Quasar's `rules` prop on `QInput`/`QSelect`.

#### Lens 5: Content and writing (findings are usually Medium)

Distilled from `writing.md`:

- Every label says what happens: "Save changes", not "Submit". An action keeps its name through the
  whole flow.
- Capitalization follows the platform (title-style for buttons, menu items, titles).
- Errors say what went wrong and how to fix it. Empty screens invite the next action.
- Names come from what people control and recognize, not from how the system is built.
- No jargon, no filler, one job per element.

### Step 4: Write the report

```text
## Design review: <name>

### Summary
Two or three sentences. Overall rating: Excellent / Good / Needs work / Critical issues.
Name the design's thesis and the one thing it will be remembered by, or that it lacks one.

### Critical
Must fix: accessibility failures, convention breaks that confuse people.
- **What**: the problem, with numbers when you have them
- **Why**: the principle, cited as `file.md › Heading` with a short quote
- **Fix**: the concrete Quasar change (component name, props, CSS class)

### Improvements
Should fix. Same format, each finding tagged High, Medium, or Low.

### Craft notes
Point of view, typography, signature element, restraint. Same format with tags, or a short
paragraph when the design is strong.

### What works
Patterns to keep. Be specific so they survive the next iteration.

### Platform notes
Anything specific to mobile versus desktop, Capacitor versus Electron versus SPA, or Quasar version.
```

Include only the sections that have content; Summary always appears. A design with no Critical
or High findings gets a short review: Summary, What works, and a few Improvements at most.

Severity, tagged on every finding:

- **Critical**: accessibility failures, unusable on some devices or sizes, conventions broken in
  ways that confuse.
- **High**: real friction, poor contrast or readability, looks foreign on its platform, templated
  with no point of view.
- **Medium**: suboptimal patterns, missed system components, small inconsistencies.
- **Low**: polish and edge cases.

## Specialized review modes

- **Accessibility audit**. `accessibility.md`, `typography.md`, `color.md`. Walk every item in Lens 1,
  then screen reader order (`tabindex`, ARIA attributes), keyboard-only paths, focus management in
  dialogs (`QDialog` should trap focus).
- **Dark mode**. `dark-mode.md`, `color.md`. Semantic colors, softened whites, both appearances
  tested via `quasar.config.js` `dark: { type: 'media' }` or manual toggle.
- **Liquid Glass in Quasar**. `liquid-glass.md`, `materials.md`. Use `backdrop-filter: blur()` with
  vendor prefixes and fallback background. Apply only to floating layers (`QToolbar` translucent,
  `QPageSticky` bars).
- **Navigation structure**. `tab-bars.md`, `sidebars.md`, `split-views.md`. Map the hierarchy, count
  tabs, check that every section is reachable, and that tablet/desktop widths convert sensibly.
- **Onboarding and permissions**. `onboarding.md`, `launching.md`, `privacy.md`. Launch instantly,
  teach through use, delay sign-in, ask for permission in context with an honest purpose string.
- **Forms and data entry**. `entering-data.md`, `text-fields.md`, `pickers.md`. Right input type,
  hints in fields, dynamic validation via Quasar `rules`, sensible tab order.
- **Generative AI UX**. `generative-ai.md`, `machine-learning.md`. Disclosure, expectations,
  control, refine and revert, hallucination awareness.

## Design improvement mode

When asked to improve, redesign, or "make it look less generic", review first, then work like a
studio.

1. **Ground it in the subject.** Name the product, its audience, and the screen's single job.
2. **Plan a compact token system** before touching layout:
   - **Color**: define Quasar palette entries (`$primary`, `$secondary`, `$accent`, `$negative`) with
     light and dark variants in `quasar.config.js`. Include contrast ratios against surfaces.
   - **Type**: body text at or above platform minimums. Use Quasar's typography system or custom
     SCSS mixins. Show the scale.
   - **Layout**: one sentence and an ASCII wireframe of the key screen, mapped to Quasar layout
     (`QPage`, `QToolbar`, `QTabs`, `QDrawer`).
   - **Signature**: the single element the design will be remembered by.
3. **Critique the plan.** Would you have produced this same plan for a similar brief about a different
   product? Then it is a default. Revise it.
4. **Propose fixes as concrete changes** in Quasar: exact colors, exact type styles, the named Quasar
   component with props, the CSS class or SCSS variable to set. Not "fix the contrast" but
   "body text from `$grey-7` (`#757575`) to `$grey-9` (`#212121`) on `white`, 12.6:1".
5. **Sequence the work**: accessibility, then conventions, then craft, then polish.

## Working rules

- **Numbers, not adjectives.** "14px #757575 on white, 4.6:1" beats "hard to read".
- **Cite it or label it as judgment.** Never invent a guideline.
- **Speak Quasar.** `QBtn unelevated color="primary"`, not just "button primary".
- **Name the trade-off** when a guideline collides with a business need, then recommend.
- **Review the flow, not just the screen.** A fine screen can break the navigation around it.
- **Don't over-critique.** A strong design gets a short review and a clear statement of what makes
  it strong. Not every review needs twenty findings.
