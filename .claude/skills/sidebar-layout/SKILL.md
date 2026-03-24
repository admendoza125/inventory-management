---
name: sidebar-layout
description: Redesigns the Vue 3 app UI from a horizontal top-nav to a modern SaaS-style vertical sidebar. Delegates all .vue edits to vue-expert. Verifies with Playwright.
---

# Skill: Sidebar Layout Redesign

Redesign the Factory Inventory Management System frontend from a horizontal sticky top-nav layout to a modern SaaS-style vertical sidebar layout. This skill modifies exactly two files: `client/src/App.vue` and `client/src/components/FilterBar.vue`.

---

## MANDATORY DELEGATION RULE

ANY modification to a `.vue` file MUST be delegated to the `vue-expert` subagent. Do not directly edit `.vue` files yourself. Use the Agent tool with `subagent_type: vue-expert` for all code changes in this skill.

---

## Overview of Changes

### What changes
1. **`client/src/App.vue`** — Remove the `<header class="top-nav">` element entirely. Replace the `<div class="app">` flex-column layout with a flex-row layout: a fixed sidebar on the left and a scrollable main panel on the right. Move the logo, nav links, `<LanguageSwitcher />`, and `<ProfileMenu />` into the sidebar. Rewrite all layout and nav-specific CSS.

2. **`client/src/components/FilterBar.vue`** — Remove `top: 70px` from `.filters-bar` (change to `top: 0`). Remove `max-width` from `.filters-container`. Template and script blocks are untouched.

### What does NOT change
- The `<script>` block in `App.vue` (tasks, auth, modals) — unchanged verbatim
- `<ProfileDetailsModal />` and `<TasksModal />` in `App.vue` — unchanged
- All global CSS utility classes in `App.vue` (`.card`, `.badge`, `.stat-card`, `.stats-grid`, `.table-container`, etc.) — preserved verbatim
- `client/src/main.js` — not touched
- Any view component under `client/src/views/` — not touched
- Any composable — not touched

---

## File 1: `client/src/App.vue`

### Template

Replace the entire `<template>` block with:

```vue
<template>
  <div class="app">
    <aside class="sidebar">
      <div class="sidebar-header">
        <div class="sidebar-logo">
          <h1>{{ t('nav.companyName') }}</h1>
          <span class="sidebar-subtitle">{{ t('nav.subtitle') }}</span>
        </div>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" :class="{ active: $route.path === '/' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <rect x="2" y="2" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          {{ t('nav.overview') }}
        </router-link>

        <router-link to="/inventory" :class="{ active: $route.path === '/inventory' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M2 5L9 2L16 5V13L9 16L2 13V5Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M9 2V16" stroke="currentColor" stroke-width="1.5"/>
            <path d="M2 5L16 5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          {{ t('nav.inventory') }}
        </router-link>

        <router-link to="/orders" :class="{ active: $route.path === '/orders' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M3 3H15L13 11H5L3 3Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <circle cx="6" cy="14.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="12" cy="14.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          {{ t('nav.orders') }}
        </router-link>

        <router-link to="/spending" :class="{ active: $route.path === '/spending' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M9 2C5.13 2 2 5.13 2 9C2 12.87 5.13 16 9 16C12.87 16 16 12.87 16 9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M9 5V9L12 11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M14 2V6M12 4H16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          {{ t('nav.finance') }}
        </router-link>

        <router-link to="/demand" :class="{ active: $route.path === '/demand' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <polyline points="2,13 6,8 10,10 14,5 16,7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          {{ t('nav.demandForecast') }}
        </router-link>

        <router-link to="/reports" :class="{ active: $route.path === '/reports' }">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M4 2H11L14 5V16H4V2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M11 2V5H14" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 9H11M7 12H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          Reports
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <div class="main-panel">
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>
```

### CSS in `<style>` (global, unscoped)

**Remove these rule blocks entirely** (they belong to the old top-nav):
- `.top-nav { … }`
- `.nav-container { … }`
- `.logo { … }` and `.logo h1 { … }`
- `.subtitle { … }`
- `.nav-tabs { … }`, `.nav-tabs a { … }`, `.nav-tabs a:hover { … }`, `.nav-tabs a.active { … }`, `.nav-tabs a.active::after { … }`
- The existing `.main-content { … }` rule (replaced below)
- The existing `.app { … }` rule (replaced below)

**Replace `.app` with:**
```css
.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
}
```

**Add these new rules:**
```css
/* ── Sidebar ───────────────────────────────────────────────── */
.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: 240px;
  height: 100vh;
  background: #0f172a;
  display: flex;
  flex-direction: column;
  z-index: 100;
  overflow: hidden;
}

.sidebar-header {
  padding: 1.5rem 1.25rem 1rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  flex-shrink: 0;
}

.sidebar-logo h1 {
  font-size: 1rem;
  font-weight: 700;
  color: #f8fafc;
  letter-spacing: -0.02em;
  line-height: 1.3;
  margin-bottom: 0.25rem;
}

.sidebar-subtitle {
  font-size: 0.75rem;
  color: #64748b;
  font-weight: 400;
}

/* ── Sidebar Nav ─────────────────────────────────────────────── */
.sidebar-nav {
  flex: 1;
  padding: 1rem 0.75rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  overflow-y: auto;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.875rem;
  color: #94a3b8;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.875rem;
  border-radius: 6px;
  transition: background 0.15s ease, color 0.15s ease;
  border-left: 2px solid transparent;
}

.sidebar-nav a:hover {
  background: rgba(255, 255, 255, 0.06);
  color: #e2e8f0;
}

.sidebar-nav a.active {
  background: rgba(37, 99, 235, 0.18);
  color: #93c5fd;
  border-left-color: #3b82f6;
}

.sidebar-nav a svg {
  flex-shrink: 0;
  opacity: 0.8;
}

.sidebar-nav a.active svg {
  opacity: 1;
}

/* ── Sidebar Footer ──────────────────────────────────────────── */
.sidebar-footer {
  padding: 1rem 0.75rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex-shrink: 0;
}

/* Override LanguageSwitcher + ProfileMenu for dark sidebar background */
.sidebar-footer .language-button,
.sidebar-footer .profile-button {
  width: 100%;
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.1);
  color: #94a3b8;
  justify-content: flex-start;
}

.sidebar-footer .language-button:hover,
.sidebar-footer .profile-button:hover {
  background: rgba(255, 255, 255, 0.09);
  border-color: rgba(255, 255, 255, 0.15);
  color: #e2e8f0;
}

.sidebar-footer .globe-icon {
  color: #64748b;
}

.sidebar-footer .profile-name {
  color: #cbd5e1;
}

.sidebar-footer .chevron {
  color: #475569;
}

/* Reposition dropdowns upward so they don't clip below viewport */
.sidebar-footer .dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  left: 0;
  right: auto;
}

/* ── Main Panel ──────────────────────────────────────────────── */
.main-panel {
  margin-left: 240px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  min-width: 0;
}

.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
}
```

---

## File 2: `client/src/components/FilterBar.vue`

Only the `<style scoped>` block changes. Template and script are untouched.

**In `.filters-bar`**, change `top: 70px` → `top: 0`:
```css
/* BEFORE */
.filters-bar {
  ...
  top: 70px;
  ...
}

/* AFTER */
.filters-bar {
  ...
  top: 0;
  ...
}
```

**In `.filters-container`**, remove the `max-width` and `margin: 0 auto` lines:
```css
/* BEFORE */
.filters-container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 2rem;
  ...
}

/* AFTER */
.filters-container {
  padding: 0 2rem;
  ...
}
```

---

## Execution Instructions for vue-expert

1. Read `client/src/App.vue` in full before making any changes.
2. Read `client/src/components/FilterBar.vue` in full before making any changes.
3. Apply template and CSS changes to `client/src/App.vue` as specified. Do NOT alter the `<script>` block.
4. Apply CSS-only changes to `client/src/components/FilterBar.vue`. Do NOT alter `<template>` or `<script>`.
5. After writing both files, proceed to the Verification step.

---

## Verification with Playwright

Verify at `http://localhost:3001` (try `localhost:3000` if 3001 returns no response).

### Step 1 — Full-page screenshot
Navigate to the root URL. Take a screenshot. Confirm:
- A dark sidebar (~240px) is visible on the left
- No horizontal top nav bar is present
- Six nav links appear in the sidebar
- Filter bar appears at the top of the main (right) content area

### Step 2 — Active nav state
At `/`, confirm the "Overview" link has a visually distinct active state (blue-tinted background, left border accent).

### Step 3 — Navigate to Inventory
Click the Inventory sidebar link. Confirm URL changes to `/inventory`, active state moves to Inventory, Overview is no longer active, and content updates.

### Step 4 — Sticky filter bar while scrolling
Navigate to `/orders`. Scroll down 400px. Confirm the filter bar stays visible at the top of the main panel and the sidebar remains fixed.

### Step 5 — Profile dropdown
Click the profile button in the sidebar footer. Confirm the dropdown opens above (not below) the button and is readable.

### Step 6 — All six routes
Navigate through all six links. Confirm each renders without a JS error visible in the page body and the active nav link updates correctly.

**Pass criteria:** All six steps pass. If any fails, diagnose, fix, and re-run the failing steps.

---

## Design Spec Reference

| Token | Value |
|---|---|
| Sidebar width | `240px` |
| Sidebar bg | `#0f172a` |
| Sidebar divider | `rgba(255,255,255,0.07)` |
| Nav link default | `#94a3b8` |
| Nav link hover | `#e2e8f0` |
| Nav link active text | `#93c5fd` |
| Nav link active bg | `rgba(37,99,235,0.18)` |
| Nav link active border | `#3b82f6` |
| Logo text | `#f8fafc` |
| Logo subtitle | `#64748b` |
| Sidebar header padding | `1.5rem 1.25rem 1rem` |
| Nav padding | `1rem 0.75rem` |
| Nav link padding | `0.625rem 0.875rem` |
| Main content padding | `1.5rem 2rem` |
| Min viewport | `1280px` (no mobile breakpoints required) |

---

## Common Mistakes to Avoid

- Do NOT set `top: 70px` anywhere — that offset was for the old top-nav
- Do NOT add `overflow: hidden` to `.main-panel` — views must scroll vertically
- Do NOT change the `<script>` in `App.vue`
- Do NOT add `max-width` or `margin: 0 auto` to `.main-content`
- Do NOT edit `ProfileMenu.vue` or `LanguageSwitcher.vue` directly — override their classes from App.vue's global style block
- The `.sidebar-footer .dropdown-menu` override positions dropdowns upward to prevent clipping at the bottom of the viewport
