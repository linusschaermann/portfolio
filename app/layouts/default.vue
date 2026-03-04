<script setup lang="ts">
import type { NavigationMenuItem } from '@nuxt/ui'
import { SpeedInsights } from '@vercel/speed-insights/nuxt'

// const labels = ['Home', 'Projects', 'Research', 'Contact']
const labels = ['Home', 'Contact']
const activeIndex = useState('activeIndex', () => 0)

defineShortcuts({
  1: () => { activeIndex.value = 0 },
  2: () => { activeIndex.value = 1 }
  // 3: () => { activeIndex.value = 2 },
  // 4: () => { activeIndex.value = 3 }
})

const open = ref(false)

const items = computed<NavigationMenuItem[]>(() =>
  labels.map((label, i) => ({
    label,
    slot: label.toLowerCase(),
    active: i === activeIndex.value,
    onSelect: () => { activeIndex.value = i }
  }))
)
</script>

<template>
  <div>
    <SpeedInsights />
    <UDashboardGroup unit="rem">
      <UDashboardSidebar
        id="default"
        v-model:open="open"
        :min-size="12"
        collapsible
        resizable
        class="border-r-0 py-4"
      >
        <template #header="{ collapsed }">
          <div class="flex items-center justify-between gap-2 w-full">
            <h1 class="font-bold text-lg">
              {{ collapsed ? 'LS.' : 'Linus Schärmann.' }}
            </h1>
          </div>
        </template>
        <template #default="{ collapsed }">
          <UNavigationMenu
            v-if="!collapsed"
            :items="items"
            :collapsed="collapsed"
            orientation="vertical"
            :ui="{ link: 'overflow-hidden' }"
          >
            <template
              v-for="(label, i) in labels"
              :key="label"
              #[`${label.toLowerCase()}-trailing`]
            >
              <UKbd :value="String(i + 1)" />
            </template>
          </UNavigationMenu>
        </template>
      </UDashboardSidebar>

      <div class="flex-1 flex m-4 lg:ml-0 rounded-lg ring ring-muted bg-muted/75 shadow min-w-0 min-h-0">
        <slot />
      </div>
    </UDashboardGroup>
  </div>
</template>
