# Quasar Design Skill

An AI design reviewer grounded in Apple's Human Interface Guidelines, translated for **Quasar
Framework** (Vue 3). It audits and improves Quasar apps — SPA, PWA, Electron, Capacitor — against
Apple's eight design principles and five audit lenses, and every finding is expressed as a concrete
Quasar component change (`QBtn`, `QTabs`, `QDialog`, etc.).

> **Attribution**: The underlying skill, Apple HIG reference files, review methodology, five-lens
> audit process, and report format are from [apple-design-skill](https://github.com/dickwu/apple-design-skill)
> by **Dick Wu** ([@dickwu](https://github.com/dickwu)). This repository contains only the
> **Quasar-specific adaptation**: component mappings, Quasar code examples, Liquid Glass patterns
> for Quasar's CSS architecture, and build-target considerations. All original skill content — the
> design principles, audit lenses, review process, improvement mode, and Apple HIG references —
> belongs to Dick Wu.

## Install

### With the skills CLI

```bash
npx skills add alainrc2005/quasar-design-skill
```

This works for Claude Code, Codex, Cursor, OpenCode, Windsurf, and every other agent the
[skills CLI](https://skills.sh) supports. Useful variations:

```bash
npx skills add alainrc2005/quasar-design-skill -a claude-code   # install for one agent only
npx skills add alainrc2005/quasar-design-skill -g               # user-wide instead of this project
npx skills add alainrc2005/quasar-design-skill --list           # show what would be installed
npx skills update quasar-design                                # pull the latest version later
```

### By hand

Clone into your project or global skills directory:

```bash
git clone https://github.com/alainrc2005/quasar-design-skill.git ~/.claude/skills/quasar-design
```

The skill triggers on design-review requests and is also available as `/quasar-design`.

### Cursor

Install with `npx skills add alainrc2005/quasar-design-skill -a cursor`, or clone the repository and
keep `.cursorrules` next to your own rules. Then ask Cursor: *"Review this Quasar screen against
Apple's HIG"*.

## Using this skill in your project

There are two ways to use this skill effectively: **with an AI agent** (Claude Code, Cursor, etc.)
and **manually** as a reference checklist.

### Method 1: With an AI agent (recommended)

The skill works by providing the agent with structured guidelines and component mappings before it
reviews your code or design. Here's how to set it up:

#### Step 1 — Install the skill

Follow one of the installation methods above. The skill files must be accessible to your agent:

- **Claude Code**: `~/.claude/skills/quasar-design/` or inside your project at `.claude/skills/quasar-design/`
- **Cursor**: keep `.cursorrules` next to your project rules
- **Other agents**: point their rules file at `SKILL.md` in this repository

#### Step 2 — Point the agent to your Quasar code

When you ask for a review, provide context about your project:

```
Review my Quasar app's home screen. It's built with Quasar v2 + Vue 3, targeting SPA mode.
The build target is PWA. Here's the main view component:

<details>
<summary>src/pages/Index.vue</summary>

{{ paste your code here }}
</details>
```

Or share a screenshot along with the code:

```
Review this screen against Apple's HIG and Quasar best practices.
[attach screenshot]
```

#### Step 3 — Ask targeted questions

The skill covers many areas. Be specific for better results:

| Prompt | What it triggers |
|---|---|
| "Review my home screen" | Full audit through all 5 lenses |
| "Audit this for accessibility" | Lens 1 only + screen reader order |
| "Check my dark mode implementation" | Dark mode specialized review |
| "Is my QTabs implementation HIG-compliant?" | Navigation lens + tab-bars.md |
| "How do I implement Liquid Glass in a QToolbar?" | liquid-glass-quasar.md patterns |
| "Make this look less generic" | Improvement mode with token system |
| "Check my forms against HIG" | Data entry lens + text-fields, pickers |

#### Step 4 — Apply the fixes

The agent returns findings in this format:

```markdown
### Critical

- **What**: Body text contrast is 3.2:1 (#888 on white), below the 4.5:1 minimum.
- **Why**: `color.md › Contrast` — "Text up to 17px needs 4.5:1."
- **Fix**: Change body text from `$grey-7` (`#757575`) to `$grey-9` (`#212121`) in your
  `quasar.config.js` palette, or use the Quasar class `text-dark`.

### Improvements (High)

- **What**: QBtn for primary action uses `flat` style instead of `unelevated`.
- **Why**: `buttons.md › Style` — "Use a prominent button style for the primary action."
- **Fix**: Change `<q-btn flat label="Save" color="primary">` to
  `<q-btn unelevated label="Save" color="primary">`.
```

Apply the Quasar-specific fixes directly. The agent always gives you the exact component, props,
and CSS class to change.

### Method 2: Manual review as a checklist

If you're not using an AI agent, use the files as a manual reference:

1. Open `references/quasar-components.md` and find the category that matches what you're building
   (Navigation, Actions, Modality, etc.).
2. Check each compliance checkbox for your component.
3. Cross-reference with the Apple HIG file linked in each section for the full guideline text.
4. Use `references/liquid-glass-quasar.md` if you need glass/translucent effects.

## Use it

Ask your agent things like:

- *"Review my Quasar app's home screen against Apple's guidelines"*
- *"Audit this Electron app built with Quasar for accessibility"*
- *"Does this settings screen follow macOS conventions in Quasar?"*
- *"How do I implement Liquid Glass in a Quasar QToolbar?"*
- *"This looks generic. Give it a point of view without breaking iOS patterns"*
- *"Check my QTabs implementation against Apple's tab bar guidelines"*
- *"Is this onboarding flow asking for permissions the right way?"*

## What a review covers

The reviewer reads the relevant guideline files before it writes a word, then works through:

1. **Apple's eight design principles** (purpose, agency, responsibility, familiarity, flexibility,
   simplicity, craft, delight).
2. **Accessibility**, blocking: text scaling, contrast ratios, control sizes, screen readers,
   keyboard access, motion and transparency settings — with Quasar component-level fixes.
3. **Platform conventions**: tab bars (`QTabs`), toolbars (`QToolbar`), sheets (`QDialog`), search,
   and safe areas on mobile; menu bar, windows, sidebars (`QDrawer`), shortcuts on desktop.
4. **Visual design and craft**: color (Quasar palette), type, layout, icons, materials, motion —
   translated to Quasar props and CSS.
5. **Interaction**: loading (`QSpinner`), feedback (`QBanner`), alerts, modality, destructive actions,
   undo, data entry (`QInput` with `rules`).
6. **Content and writing**: labels that say what happens, platform-correct capitalization,
   errors and empty states that direct people.

Every finding has a What, a Why that cites the guideline file and heading, and a Fix written in
Quasar vocabulary: component name, props, CSS class, or `quasar.config.js` setting. Specialized
modes cover accessibility audits, dark mode, Liquid Glass, navigation structure, onboarding, forms,
and single components.

## What's inside

```text
quasar-design-skill/
├── SKILL.md                     # The skill: stance, principles, lenses, report format
├── AGENTS.md                    # Operating manual for agents and contributors
├── .cursorrules                 # Cursor entry point
└── references/
    ├── quasar-components.md     # Apple HIG → Quasar component mapping with props
    ├── liquid-glass-quasar.md   # Curated: Liquid Glass patterns for Quasar
    ├── hig-lookup.md            # Routing table to Apple's guidelines
    └── hig/                     # Apple's HIG pages (symlinked from apple-design-skill)
        ├── accessibility.md
        ├── designing-for-ios.md
        ├── buttons.md
        ├── tab-bars.md
        └── ...
```

## Relationship to apple-design-skill

This is a **Quasar-specific adaptation** of the [apple-design-skill](https://github.com/dickwu/apple-design-skill)
by Dick Wu. The original skill's content — design principles, five-lens audit process, report format,
improvement mode, and all Apple HIG reference files — is unchanged.

Our contribution is exclusively:

- **`references/quasar-components.md`** — complete mapping of every Apple HIG UI concept to the
  corresponding Quasar component, with Vue 3 code examples, compliance checklists, and props for
  HIG compliance.
- **`references/liquid-glass-quasar.md`** — curated Liquid Glass patterns adapted for Quasar's CSS
  architecture, including dark mode, reduced transparency, and Capacitor/Electron/PWA compatibility.
- **Build-target considerations** — notes specific to SPA, PWA, Electron, and Capacitor (iOS/Android)
  build targets within the skill content.

The Apple HIG files are shared via symlink to avoid duplication. When the parent repository updates
its guidelines, this skill automatically reflects those changes.

## Keeping the references current

To refresh the Apple HIG files from the parent repository:

```bash
# If using symlinks (default), re-point them after updating apple-design-skill
rm -f references/hig references/hig-lookup.md
ln -sf /absolute/path/to/apple-design-skill/references/hig references/hig
ln -sf /absolute/path/to/apple-design-skill/references/hig-lookup.md references/hig-lookup.md

# Or pull from the parent repo if using a submodule
cd .deps/apple-design-skill && git pull && cd ..
```

## Sponsor

If this skill saves you review time, you can support its upkeep through
the [apple-design-skill sponsor page](https://github.com/sponsors/dickwu). One-time and monthly
tiers are available.

## Origin and license

The underlying skill, Apple HIG reference files, review methodology, five-lens audit process, and
report format belong to **Dick Wu** ([@dickwu](https://github.com/dickwu)) and are from the
[apple-design-skill](https://github.com/dickwu/apple-design-skill) repository.

The guideline text belongs to Apple Inc. and is reproduced from the public
[Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
for AI-assisted design review, with a source link at the top of every file. This project is not
affiliated with or endorsed by Apple.

The Quasar-specific additions — component mappings, Liquid Glass patterns for Quasar, and build-target
considerations — are provided as they are; use them at your own discretion.

Quasar Framework is developed by [rstacruz](https://github.com/rstacruz) and contributors under the
MIT license. See https://quasar.dev for details.
