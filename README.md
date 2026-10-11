# Quasar Design Skill

An AI design reviewer grounded in Apple's Human Interface Guidelines, translated for **Quasar
Framework** (Vue 3). It audits and improves Quasar apps — SPA, PWA, Electron, Capacitor — against
Apple's eight design principles and five audit lenses, and every finding is expressed as a concrete
Quasar component change (`QBtn`, `QTabs`, `QDialog`, etc.).

## Install

### With the skills CLI

```bash
npx skills add <username>/quasar-design-skill
```

This works for Claude Code, Codex, Cursor, OpenCode, Windsurf, and every other agent the
[skills CLI](https://skills.sh) supports. Useful variations:

```bash
npx skills add <username>/quasar-design-skill -a claude-code   # install for one agent only
npx skills add <username>/quasar-design-skill -g               # user-wide instead of this project
npx skills add <username>/quasar-design-skill --list           # show what would be installed
npx skills update quasar-design                                # pull the latest version later
```

### By hand

Clone into your project or global skills directory:

```bash
git clone https://github.com/<username>/quasar-design-skill.git ~/.claude/skills/quasar-design
```

The skill triggers on design-review requests and is also available as `/quasar-design`.

### Cursor

Install with `npx skills add <username>/quasar-design-skill -a cursor`, or clone the repository and
keep `.cursorrules` next to your own rules. Then ask Cursor: *"Review this Quasar screen against
Apple's HIG"*.

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
    └── hig/                     # Apple's HIG pages (copied/symlinked from apple-design-skill)
        ├── accessibility.md
        ├── designing-for-ios.md
        ├── buttons.md
        ├── tab-bars.md
        └── ...
```

## Relationship to apple-design-skill

This skill is a **Quasar-specific fork** of the [apple-design-skill](https://github.com/dickwu/apple-design-skill)
repository. It shares:

- The same Apple HIG reference files (via symlink or submodule)
- The same five-lens review process and report format
- The same design principles and improvement mode

It adds:

- A complete component mapping (`references/quasar-components.md`) covering all Quasar UI components
- Liquid Glass patterns adapted for Quasar's CSS architecture
- Quasar-specific compliance checklists (props, breakpoints, theme config)
- Capacitor, Electron, PWA, and SPA build-target considerations

## Keeping the references current

To refresh the Apple HIG files from the parent repository:

```bash
# If using symlinks, re-point them after updating apple-design-skill
ln -sf ../apple-design-skill/references/hig references/hig
ln -sf ../apple-design-skill/references/hig-lookup.md references/hig-lookup.md

# Or pull from the parent repo
cd .deps/apple-design-skill && git pull && cd ..
```

## Sponsor

If this skill saves you review time, you can support its upkeep through
the [apple-design-skill sponsor page](https://github.com/sponsors/dickwu). One-time and monthly
tiers are available.

## Origin and license

The guideline text belongs to Apple Inc. and is reproduced from the public
[Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
for AI-assisted design review, with a source link at the top of every file. This project is not
affiliated with or endorsed by Apple. The skill, the curated guides, and the component mappings are
provided as they are; use them at your own discretion.

Quasar Framework is developed by [rstacruz](https://github.com/rstacruz) and contributors under the
MIT license. See https://quasar.dev for details.
