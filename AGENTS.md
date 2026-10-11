# Quasar Design Skill — Operating Manual

## What this repository is

Apple Design Skill for Quasar: a design-review skill that translates Apple's Human Interface
Guidelines into Quasar Framework (Vue 3) component patterns. It audits and improves Quasar apps
(SPAs, PWAs, Electron, Capacitor) against Apple's eight design principles and five audit lenses,
and every finding is expressed in Quasar vocabulary (`QBtn`, `QTabs`, `QDialog`, etc.).

## Running a design review

1. Read `SKILL.md` in full. It defines the reviewer's stance, Apple's eight design principles,
   the five audit lenses, the report format, and the improvement process — all translated for Quasar.
2. Open `references/quasar-components.md` to find the Quasar component that implements each HIG
   concept, with props and compliance checklists.
3. Load `references/hig/accessibility.md`, `layout.md`, `typography.md`, `color.md`, plus
   `designing-for-ios.md` or `designing-for-macos.md` for the platform in front of you.
4. Load 3 to 6 more files from `references/hig/` for what is on screen (see the loading table in
   `SKILL.md`). Never load the whole directory.
5. Cite the guideline as `file.md › Heading` and give the fix as a Quasar component change
   (component name, props, CSS class).

## File map

| Path | Role | Edit by hand? |
| --- | --- | --- |
| `SKILL.md` | The skill: stance, principles, lenses, report format, improvement mode for Quasar | Yes |
| `AGENTS.md` | This file | Yes |
| `README.md` | Installation and overview | Yes |
| `.cursorrules` | Entry point for Cursor | Yes |
| `references/quasar-components.md` | Component mapping: Apple HIG → Quasar with props and checklists | Yes |
| `references/liquid-glass-quasar.md` | Curated Liquid Glass guide for Quasar | Yes |
| `references/hig-lookup.md` | Routing table to Apple's guidelines (symlink or copy from apple-design-skill) | No, update from parent |
| `references/hig/<page>.md` | Apple's HIG pages in original wording | Copy/symlink from parent repo |

## Linking to the parent skill

This skill references Apple's HIG files but does not regenerate them. You have two options:

1. **Symlink** the parent's `references/hig/` directory:
   ```bash
   ln -s ../apple-design-skill/references/hig references/hig
   ln -s ../apple-design-skill/references/hig-lookup.md references/hig-lookup.md
   ```
2. **Git submodule**:
   ```bash
   git submodule add https://github.com/dickwu/apple-design-skill.git .deps/apple-design-skill
   ln -s .deps/apple-design-skill/references/hig references/hig
   ln -s .deps/apple-design-skill/references/hig-lookup.md references/hig-lookup.md
   ```

## Checks before finishing any change

```bash
# Verify the skill loads correctly (syntax check)
node --check SKILL.md 2>&1 || true  # SKILL.md is Markdown, not JS

# Confirm every guideline file named in SKILL.md exists
ls references/hig/accessibility.md references/hig/layout.md \
   references/hig/typography.md references/hig/color.md

# Confirm Quasar component mapping covers the main components
grep -c '→' references/quasar-components.md  # should be > 30 mappings

# If you have a Quasar project to test against:
cd <quasar-project> && quasar dev  # verify no build errors with referenced patterns
```

## Conventions

- Keep `SKILL.md` under about 400 lines. It loads in full every time the skill triggers.
- Apple's text in `references/hig/` is quoted, not paraphrased. Quasar translation lives in
  `references/quasar-components.md`, `SKILL.md`, and `liquid-glass-quasar.md`.
- Commit generated changes (if any) separately from hand-written changes.
- Do not commit `.cache/`, `.omc/`, or other local state.
