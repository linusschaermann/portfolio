<template>
  <UDashboardNavbar
    class="sticky lg:absolute rounded-lg top-0 inset-x-0 border-b-0 z-10 pointer-events-none sm:px-4"
    :ui="{ left: 'pointer-events-auto', right: 'pointer-events-auto' }"
  >
    <template #left>
      <UDashboardSidebarCollapse />
    </template>

    <template #right>
      <UColorModeButton />
    </template>
  </UDashboardNavbar>
</template>
