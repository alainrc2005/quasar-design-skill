# Quasar Component Mapping — Apple HIG Concepts

This file maps every Apple HIG UI concept to the Quasar component that implements it, with the
props, patterns, and caveats needed for HIG compliance in a Quasar app (Vue 3, Quasar v2).

## Navigation

### Tab Bar → QTabs

Apple says: "Use a tab bar to support navigation, not to provide actions." (`tab-bars.md`)

```vue
<template>
  <q-tabs
    v-model="tab"
    dense
    class="text-grey"
    active-color="primary"
    indicator-color="primary"
    align="justify"
    narrow-label
    :class="{ 'desktop-sidebar': $q.screen.gt sm }"
  >
    <q-route-tab name="home" label="Home" icon="home" />
    <q-route-tab name="search" label="Search" icon="search" />
    <q-route-tab name="profile" label="Profile" icon="person" />
  </q-tabs>
</template>
```

Compliance checklist:
- [ ] Each tab has an icon + short label (single word where possible)
- [ ] Filled symbols preferred over outlined for active state
- [ ] Few tabs (3–5); overflow handled via "More" or scrollable tabs
- [ ] Tabs navigate (`q-route-tab`), they don't trigger actions
- [ ] Active tab has clear visual distinction (active-color + indicator-color)

Responsive behavior: on tablet/desktop, convert the tab bar to a sidebar using `QDrawer` with
`QItem` links. Use Quasar's `$q.screen` breakpoint (`gt-sm` or `gt-md`) to switch.

### Sidebar → QDrawer

Apple says: "Use a sidebar to provide navigation between top-level sections." (`sidebars.md`)

```vue
<template>
  <q-drawer
    v-model="drawerOpen"
    side="left"
    :width="260"
    behavior="desktop"
    class="bg-grey-2"
    show-if-above
  >
    <q-list>
      <q-item clickable v-ripple v-for="item in navItems" :key="item.label">
        <q-item-section avatar>
          <q-icon :name="item.icon" />
        </q-item-section>
        <q-item-section>{{ item.label }}</q-item-section>
      </q-item>
    </q-list>
  </q-drawer>
</template>
```

Compliance checklist:
- [ ] At most two levels of hierarchy in the sidebar
- [ ] Sidebar can be hidden (collapsible via `v-model`)
- [ ] Critical actions are NOT at the bottom of the sidebar
- [ ] Current location is always visible and highlighted
- [ ] Keyboard focusable items with proper `tabindex`

### Toolbar → QToolbar

Apple says: "Place key actions in a toolbar." (`toolbars.md`)

```vue
<template>
  <q-toolbar class="bg-white text-dark">
    <q-btn flat dense round icon="menu" @click="drawer = !drawer" aria-label="Menu" />
    <q-toolbar-title>{{ title }}</q-toolbar-title>
    <q-space />
    <q-btn flat dense round icon="search" aria-label="Search" />
    <q-btn flat dense round icon="more_vert" aria-label="More" />
  </q-toolbar>
</template>
```

Compliance checklist:
- [ ] Key actions (Done, Save, Delete) are in the toolbar
- [ ] Primary action uses prominent style (`color="primary"` or `unelevated`)
- [ ] Toolbar stays lightly tinted and monochrome over colorful content
- [ ] Overflow goes into a "More" menu, never hidden behind gestures

### Back Navigation

Apple says: "Provide an obvious way to go back." (`toolbars.md`)

In Quasar: use Vue Router's `router.back()` or the browser's history API. For custom implementations,
use a chevron icon button in the leading slot of `QToolbar`. Never rely solely on swipe-back; always
provide a visible back control.

### Split View → QSplitter

Apple says: "Use split views to show related content side by side." (`split-views.md`)

```vue
<template>
  <q-splitter v-model="splitRatio" :limits="[20, 80]">
    <template v-slot:before>
      <q-list>
        <!-- Sidebar / list panel -->
      </q-list>
    </template>
    <template v-slot:after>
      <q-page-container>
        <!-- Detail panel -->
      </q-page-container>
    </template>
  </q-splitter>
</template>
```

Compliance checklist:
- [ ] Minimum sizes set via `:limits` prop
- [ ] Both panels are accessible independently
- [ ] On narrow screens, collapse to a single panel with navigation between them

## Actions

### Primary Button → QBtn (unelevated / elevated)

Apple says: "Use a prominent button style for the primary action." (`buttons.md`)

```vue
<!-- Primary action -->
<q-btn label="Save Changes" color="primary" unelevated class="full-width" />

<!-- Secondary action -->
<q-btn label="Cancel" flat class="full-width" />

<!-- Destructive action -->
<q-btn label="Delete" color="negative" unelevated class="full-width" />
```

Compliance checklist:
- [ ] Primary action is visually distinct (filled/unelevated, primary color)
- [ ] Secondary actions are flat or outline
- [ ] Destructive actions use `color="negative"`
- [ ] Button label describes the outcome ("Save Changes", not "Submit")
- [ ] Minimum 44px touch target on mobile (`min-height: 44px` in CSS if needed)

### Icon Button → QBtn (round flat)

```vue
<q-btn round flat icon="favorite" aria-label="Favorite" />
```

Compliance checklist:
- [ ] Always accompanied by a label for screen readers (aria-label or visible text)
- [ ] Minimum 44px hit area on mobile
- [ ] Icon conveys meaning beyond color alone

### Menu → QMenu / QBtnDropdown

Apple says: "Menus provide a list of commands related to the current view." (`menus.md`)

```vue
<template>
  <q-btn-dropdown label="More" icon="more_vert">
    <q-list>
      <q-item clickable v-close-popup>
        <q-item-section>Settings</q-item-section>
      </q-item>
      <q-item clickable v-close-popup>
        <q-item-section>About</q-item-section>
      </q-item>
    </q-list>
  </q-btn-dropdown>
</template>
```

### Segmented Control → QBtnToggle

```vue
<template>
  <q-btn-toggle
    v-model="viewMode"
    toggle-color="primary"
    unelevated
    rounded
    spread
    options="[
      { label: 'List', value: 'list', icon: 'list' },
      { label: 'Grid', value: 'grid', icon: 'grid_on' }
    ]"
  />
</template>
```

## Modality

### Sheet → QDialog with transition

Apple says: "Present sheets from the bottom on mobile." (`sheets.md`)

```vue
<template>
  <q-dialog v-model="sheetOpen" @escape-press="onEscape">
    <q-card class="sheet-dialog" style="max-width: 500px; border-top-left-radius: 12px; border-top-right-radius: 12px;">
      <!-- Grabber bar -->
      <div class="grabber-row">
        <div class="grabber" />
      </div>
      <q-card-section>
        <div class="text-h6 q-mb-sm">Title</div>
        <p>Sheet content goes here.</p>
      </q-card-section>
      <q-card-actions align="right">
        <q-btn flat label="Cancel" color="primary" v-close-popup />
        <q-btn unelevated label="Done" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<style scoped>
.sheet-dialog {
  border-top-left-radius: 12px;
  border-top-right-radius: 12px;
}
.grabber-row {
  display: flex;
  justify-content: center;
  padding: 8px 0 0;
}
.grabber {
  width: 36px;
  height: 4px;
  border-radius: 2px;
  background-color: #ccc;
}
</style>
```

Compliance checklist:
- [ ] Only one sheet open at a time
- [ ] Swipe-to-dismiss implemented (custom touch handling on `QDialog`)
- [ ] Grabber bar visible when resizable
- [ ] Medium detent considered for progressive disclosure
- [ ] A way out besides the primary action (Cancel, swipe)

### Alert → QBanner or QDialog

Apple says: "Use alerts for critical, direct messages." (`alerts.md`)

```vue
<!-- Inline banner (non-blocking) -->
<q-banner class="bg-negative text-white">
  <template v-slot:avatar>
    <q-icon name="error" color="white" />
  </template>
  Something went wrong. Please try again.
</q-banner>

<!-- Modal alert -->
<q-dialog v-model="alertOpen">
  <q-card>
    <q-card-section class="row items-center">
      <q-icon name="warning" color="warning" size="xl" />
      <div class="text-h6 q-ml-sm">Confirm Delete</div>
    </q-card-section>
    <q-card-actions align="right">
      <q-btn flat label="Cancel" color="primary" v-close-popup />
      <q-btn unelevated label="Delete" color="negative" v-close-popup />
    </q-card-actions>
  </q-card>
</q-dialog>
```

Compliance checklist:
- [ ] Alerts are rare, direct, never shown on launch
- [ ] Never used for common undoable actions (use inline feedback instead)
- [ ] Destructive irreversible actions get a warning + Cancel
- [ ] Error messages say what went wrong and how to fix it

### Popover → QMenu with anchor

```vue
<template>
  <q-btn flat label="Info" icon="info" />
  <q-menu anchor="bottom right" self="top right" :offset="[0, 8]">
    <q-card style="min-width: 200px;">
      <q-card-section>Popover content</q-card-section>
    </q-card>
  </q-menu>
</template>
```

## Data Display

### List → QList + QItem

Apple says: "Use lists to present related information in scannable rows." (`lists-and-tables.md`)

```vue
<template>
  <q-list>
    <q-item v-for="item in items" :key="item.id" clickable v-ripple @click="select(item)">
      <q-item-section avatar>
        <q-icon name="folder" />
      </q-item-section>
      <q-item-section>
        <q-item-label>{{ item.title }}</q-item-label>
        <q-item-label caption>{{ item.subtitle }}</q-item-label>
      </q-item-section>
      <q-item-section side>
        <q-badge color="primary">{{ item.badge }}</q-badge>
      </q-item-section>
    </q-item>
  </q-list>
</template>
```

Compliance checklist:
- [ ] Row height provides comfortable touch target (min 44px on mobile)
- [ ] Spacing between controls matters as much as size
- [ ] Nothing is conveyed by color alone (badges need icons or text)
- [ ] Swipe actions for common row actions (custom implementation needed)

### Card → QCard

```vue
<template>
  <q-card class="q-md">
    <q-card-section>
      <div class="text-h6">Card Title</div>
      <div class="text-body2 text-grey-8">Card content goes here.</div>
    </q-card-section>
    <q-separator />
    <q-card-actions>
      <q-btn flat label="Action" color="primary" />
      <q-btn flat label="Cancel" />
    </q-card-actions>
  </q-card>
</template>
```

### Table → QTable

```vue
<template>
  <q-table
    :rows="data"
    :columns="columns"
    row-key="id"
    dense
    virtual-scroll
    :pagination="{ rowsPerPage: 20 }"
  />
</template>
```

## Inputs

### Text Field → QInput

Apple says: "Use text fields for user input." (`text-fields.md`)

```vue
<template>
  <q-input
    v-model="value"
    label="Email address"
    outlined
    placeholder="you@example.com"
    :rules="[val => !!val || 'Email is required', val => /.+@.+\..+/.test(val) || 'Invalid email']"
    autocomplete="email"
    type="email"
  />
</template>
```

Compliance checklist:
- [ ] Label or placeholder present (not both empty)
- [ ] Right keyboard type (`type` prop, `inputmode` for numeric/telephone)
- [ ] Hints inside the field, not below it
- [ ] Dynamic validation via `rules` prop
- [ ] Never prepopulates a password

### Select / Picker → QSelect

```vue
<template>
  <q-select
    v-model="selected"
    :options="options"
    label="Choose an option"
    outlined
    clearable
    use-input
    input-debounce="300"
    option-label="label"
    option-value="value"
  />
</template>
```

### Date / Time Picker → QDate / QTime

```vue
<!-- Inline date picker -->
<q-date v-model="date" mask="YYYY-MM-DD" />

<!-- As a dialog triggered by QInput -->
<q-input v-model="date" label="Date" readonly @click="openDatePicker">
  <q-popup-proxy>
    <q-date v-model="date" mask="YYYY-MM-DD" @close="closeDatePicker" />
  </q-popup-proxy>
</q-input>
```

### Toggle → QToggle

```vue
<template>
  <div class="row items-center">
    <q-toggle v-model="enabled" color="primary" />
    <span class="q-ml-sm">Enable notifications</span>
  </div>
</template>
```

### Slider → QSlider

```vue
<template>
  <q-slider v-model="volume" label :min="0" :max="100" :step="5" color="primary" />
</template>
```

## Feedback

### Loading → QSpinner / QLinearProgress

```vue
<!-- Indeterminate -->
<q-spinner color="primary" size="xl" />

<!-- Determinate progress -->
<q-linear-progress :value="progress" color="primary" class="q-mt-md" />
```

### Notification → QNotif

```vue
import { Notify } from 'quasar'

Notify.create({
  message: 'Changes saved successfully',
  color: 'positive',
  icon: 'check_circle',
  position: 'top',
  timeout: 3000,
  actions: [{ icon: 'undo', label: 'Undo', color: 'white' }]
})
```

### Empty State → Custom template

```vue
<template>
  <q-page class="flex flex-center">
    <div class="text-center">
      <q-icon name="inbox" size="64px" color="grey-5" />
      <div class="text-h6 q-mt-md text-grey-7">No items yet</div>
      <div class="text-body2 text-grey-6 q-mb-md">Add your first item to get started.</div>
      <q-btn unelevated color="primary" label="Add Item" />
    </div>
  </q-page>
</template>
```

## Layout & Structure

### Page → QPage + QPageContainer

```vue
<template>
  <q-layout view="hHh lpR lFr">
    <q-header class="q-py-sm">
      <!-- Global header / menu bar -->
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
```

### Sticky Elements → QPageSticky

```vue
<!-- Fixed bottom action bar -->
<q-page-sticky position="bottom" class="bg-white q-pa-md">
  <q-btn unelevated color="primary" label="Save Changes" class="full-width" />
</q-page-sticky>
```

### Scrolling → QScrollArea

```vue
<template>
  <q-scroll-area style="height: 400px;">
    <!-- Long content -->
  </q-scroll-area>
</template>
```

## Quasar Theme Configuration for HIG Compliance

The following `quasar.config.js` snippet provides a starting palette aligned with Apple's color and
typography conventions:

```javascript
module.exports = function (ctx) {
  return {
    framework: {
      config: {},
      // ... other framework options
    },
    brand: {
      primary: '#007AFF',   // Apple system blue
      secondary: '#5856D6', // Apple system purple
      accent: '#FF9500',    // Apple system orange
      positive: '#34C759',  // Apple system green
      negative: '#FF3B30',  // Apple system red
      info: '#5856D6',
      warning: '#FFCC00',
    },
    dark: {
      type: 'media', // or 'manual' for a toggle
      primary: '#0A84FF',
      negative: '#FF453A',
      // ... other dark mode overrides
    },
  }
}
```

## Safe Area Handling

Apple requires content to respect safe areas. In Quasar, add this to `src/css/quasar.variables.scss`:

```scss
// Safe area insets for notched devices
$safe-top: env(safe-area-inset-top, 0px);
$safe-bottom: env(safe-area-inset-bottom, 0px);
$safe-left: env(safe-area-inset-left, 0px);
$safe-right: env(safe-area-inset-left, 0px);

// Apply to layout root
body {
  padding-top: $safe-top;
  padding-bottom: $safe-bottom;
}
```

Or use Quasar's built-in padding utilities (`q-pa-md`, etc.) combined with CSS custom properties.
