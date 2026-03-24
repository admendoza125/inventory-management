<template>
  <div class="app" :class="{ 'sidebar-collapsed': sidebarCollapsed }">
    <aside class="sidebar">
      <div class="sidebar-header">
        <div class="sidebar-logo">
          <!-- Hidden when collapsed -->
          <template v-if="!sidebarCollapsed">
            <h1>{{ t('nav.companyName') }}</h1>
            <span class="sidebar-subtitle">{{ t('nav.subtitle') }}</span>
          </template>
          <!-- Icon-only logo mark when collapsed -->
          <template v-else>
            <svg class="sidebar-logo-mark" width="24" height="24" viewBox="0 0 24 24" fill="none" aria-hidden="true">
              <rect x="3" y="3" width="8" height="8" rx="1.5" fill="#3b82f6"/>
              <rect x="13" y="3" width="8" height="8" rx="1.5" fill="#3b82f6" opacity="0.5"/>
              <rect x="3" y="13" width="8" height="8" rx="1.5" fill="#3b82f6" opacity="0.5"/>
              <rect x="13" y="13" width="8" height="8" rx="1.5" fill="#3b82f6" opacity="0.3"/>
            </svg>
          </template>
        </div>
        <!-- Toggle button -->
        <button class="sidebar-toggle" @click="toggleSidebar" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'" :aria-label="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
            <path v-if="!sidebarCollapsed" d="M10 3L5 8L10 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path v-else d="M6 3L11 8L6 13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>

      <nav class="sidebar-nav">
        <router-link to="/" :class="{ active: $route.path === '/' }" :title="sidebarCollapsed ? t('nav.overview') : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <rect x="2" y="2" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="2" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="2" y="10" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10" y="10" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>

        <router-link to="/inventory" :class="{ active: $route.path === '/inventory' }" :title="sidebarCollapsed ? t('nav.inventory') : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M2 5L9 2L16 5V13L9 16L2 13V5Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M9 2V16" stroke="currentColor" stroke-width="1.5"/>
            <path d="M2 5L16 5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>

        <router-link to="/orders" :class="{ active: $route.path === '/orders' }" :title="sidebarCollapsed ? t('nav.orders') : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M3 3H15L13 11H5L3 3Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <circle cx="6" cy="14.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="12" cy="14.5" r="1.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>

        <router-link to="/spending" :class="{ active: $route.path === '/spending' }" :title="sidebarCollapsed ? t('nav.finance') : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M9 2C5.13 2 2 5.13 2 9C2 12.87 5.13 16 9 16C12.87 16 16 12.87 16 9" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M9 5V9L12 11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M14 2V6M12 4H16" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>

        <router-link to="/demand" :class="{ active: $route.path === '/demand' }" :title="sidebarCollapsed ? t('nav.demandForecast') : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <polyline points="2,13 6,8 10,10 14,5 16,7" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>

        <router-link to="/reports" :class="{ active: $route.path === '/reports' }" :title="sidebarCollapsed ? 'Reports' : undefined">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
            <path d="M4 2H11L14 5V16H4V2Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M11 2V5H14" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 9H11M7 12H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <template v-if="!sidebarCollapsed">
          <LanguageSwitcher />
          <ProfileMenu
            @show-profile-details="showProfileDetails = true"
            @show-tasks="showTasks = true"
          />
        </template>
        <template v-else>
          <!-- Collapsed footer: just the profile avatar as an icon button -->
          <button class="sidebar-footer-icon" @click="showTasks = true" title="Tasks" aria-label="Open tasks">
            <svg width="18" height="18" viewBox="0 0 18 18" fill="none" aria-hidden="true">
              <rect x="3" y="3" width="12" height="12" rx="2" stroke="currentColor" stroke-width="1.5"/>
              <path d="M6 9H12M6 12H10M6 6H12" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            </svg>
          </button>
        </template>
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

<script>
import { ref, onMounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)

    // Sidebar collapse state — persisted to localStorage
    const sidebarCollapsed = ref(localStorage.getItem('sidebarCollapsed') === 'true')

    const toggleSidebar = () => {
      sidebarCollapsed.value = !sidebarCollapsed.value
      // Non-obvious: persist state so it survives page reloads
      localStorage.setItem('sidebarCollapsed', sidebarCollapsed.value)
    }
    const apiTasks = ref([])

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(loadTasks)

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      sidebarCollapsed,
      toggleSidebar
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ── App shell ─────────────────────────────────────────────── */
.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
}

/* ── Sidebar ────────────────────────────────────────────────── */
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
  /* Smooth width transition for collapse/expand */
  transition: width 0.22s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Collapsed state: icon-only rail */
.sidebar-collapsed .sidebar {
  width: 56px;
}

.sidebar-header {
  padding: 1.25rem 0.875rem 1rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-height: 64px;
}

/* When collapsed, center the logo mark */
.sidebar-collapsed .sidebar-header {
  justify-content: center;
  flex-direction: column;
  gap: 0.5rem;
  padding: 0.875rem 0;
}

.sidebar-logo h1 {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #f8fafc;
  letter-spacing: -0.02em;
  line-height: 1.3;
  margin-bottom: 0.2rem;
  white-space: nowrap;
}

.sidebar-subtitle {
  font-size: 0.7rem;
  color: #64748b;
  font-weight: 400;
  white-space: nowrap;
}

.sidebar-logo-mark {
  display: block;
  flex-shrink: 0;
}

/* Toggle chevron button */
.sidebar-toggle {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border: none;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 6px;
  color: #64748b;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
}

.sidebar-toggle:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #e2e8f0;
}

/* When collapsed, toggle sits below the logo mark */
.sidebar-collapsed .sidebar-toggle {
  width: 32px;
  height: 28px;
}

/* ── Sidebar Nav ────────────────────────────────────────────── */
.sidebar-nav {
  flex: 1;
  padding: 1rem 0.625rem;
  display: flex;
  flex-direction: column;
  gap: 0.125rem;
  overflow-y: auto;
  overflow-x: hidden;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  color: #94a3b8;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.875rem;
  border-radius: 6px;
  transition: background 0.15s ease, color 0.15s ease;
  border-left: 2px solid transparent;
  white-space: nowrap;
  overflow: hidden;
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

/* Nav label: fade out when collapsing */
.nav-label {
  transition: opacity 0.15s ease;
  overflow: hidden;
}

.sidebar-collapsed .nav-label {
  opacity: 0;
  width: 0;
  pointer-events: none;
}

/* When collapsed, center the icon in the link */
.sidebar-collapsed .sidebar-nav a {
  justify-content: center;
  padding: 0.625rem;
  gap: 0;
  border-left-color: transparent;
}

/* Keep active indicator as a dot on the icon when collapsed */
.sidebar-collapsed .sidebar-nav a.active {
  background: rgba(37, 99, 235, 0.22);
}

/* ── Sidebar Footer ─────────────────────────────────────────── */
.sidebar-footer {
  padding: 0.875rem 0.625rem;
  border-top: 1px solid rgba(255, 255, 255, 0.07);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  flex-shrink: 0;
}

/* Collapsed footer: centered icon buttons */
.sidebar-collapsed .sidebar-footer {
  align-items: center;
  padding: 0.875rem 0;
}

.sidebar-footer-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 36px;
  height: 36px;
  border: none;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 6px;
  color: #94a3b8;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
}

.sidebar-footer-icon:hover {
  background: rgba(255, 255, 255, 0.09);
  color: #e2e8f0;
}

/* Override LanguageSwitcher + ProfileMenu on dark sidebar */
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

/* Dropdowns open upward from footer, not clipped below viewport */
.sidebar-footer .dropdown-menu {
  bottom: calc(100% + 0.5rem);
  top: auto;
  left: 0;
  right: auto;
}

/* ── Main Panel ─────────────────────────────────────────────── */
.main-panel {
  margin-left: 240px;
  flex: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  min-width: 0;
  /* Transition matches sidebar width transition */
  transition: margin-left 0.22s cubic-bezier(0.4, 0, 0.2, 1);
}

.sidebar-collapsed .main-panel {
  margin-left: 56px;
}

.main-content {
  flex: 1;
  padding: 1.5rem 2rem;
}

.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #2563eb;
}

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: #dbeafe;
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: #dbeafe;
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>
