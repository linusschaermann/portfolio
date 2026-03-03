<script setup lang="ts">
import { ContactSection, HomeSection, ProjectsSection, ResearchSection } from '#components'

// const labels = ['Home', 'Projects', 'Research', 'Contact']
const labels = ['Home', 'Contact']

const sectionIds = labels.map(label => label.toLowerCase().replace(/\s+/g, '-'))
const sectionComponents = {
  Home: HomeSection,
  Projects: ProjectsSection,
  Research: ResearchSection,
  Contact: ContactSection
}

const activeIndex = useState('activeIndex', () => 0)

const scrollContainer = ref<HTMLElement | null>(null)
const route = useRoute()
const router = useRouter()
const syncingFromScroll = ref(false)
let sectionObserver: IntersectionObserver | null = null

const getIndexFromHash = (hash: string) => {
  const id = hash.replace(/^#/, '')
  return sectionIds.indexOf(id)
}

const scrollToActiveSection = (behavior: ScrollBehavior = 'smooth') => {
  const section = scrollContainer.value?.querySelector<HTMLElement>(
    `[data-section-index="${activeIndex.value}"]`
  )

  section?.scrollIntoView({ behavior, block: 'start' })
}

watch(activeIndex, () => {
  if (!syncingFromScroll.value) {
    scrollToActiveSection('smooth')
  }

  const hash = `#${sectionIds[activeIndex.value]}`
  if (route.hash !== hash) {
    router.replace({ hash })
  }
})

watch(() => route.hash, (hash) => {
  const nextIndex = getIndexFromHash(hash)
  if (nextIndex !== -1 && nextIndex !== activeIndex.value) {
    activeIndex.value = nextIndex
  }
})

onMounted(() => {
  const initialIndex = getIndexFromHash(route.hash)
  if (initialIndex !== -1) {
    activeIndex.value = initialIndex
  }

  scrollToActiveSection('auto')

  if (!route.hash) {
    router.replace({ hash: `#${sectionIds[activeIndex.value]}` })
  }

  const sections = scrollContainer.value?.querySelectorAll<HTMLElement>('[data-section-index]')
  if (!scrollContainer.value || !sections?.length) {
    return
  }

  sectionObserver = new IntersectionObserver(
    (entries) => {
      const mostVisibleEntry = entries
        .filter(entry => entry.isIntersecting)
        .sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0]

      if (!mostVisibleEntry) {
        return
      }

      const index = Number(
        (mostVisibleEntry.target as HTMLElement).dataset.sectionIndex
      )

      if (Number.isNaN(index) || index === activeIndex.value) {
        return
      }

      syncingFromScroll.value = true
      activeIndex.value = index

      requestAnimationFrame(() => {
        syncingFromScroll.value = false
      })
    },
    {
      root: scrollContainer.value,
      threshold: [0.5, 0.6, 0.7]
    }
  )

  sections.forEach(section => sectionObserver?.observe(section))
})

onBeforeUnmount(() => {
  sectionObserver?.disconnect()
})
</script>

<template>
  <UDashboardPanel
    class="min-h-0"
    :ui="{ body: 'p-0 sm:p-0' }"
  >
    <template #header>
      <DashboardNavbar />
    </template>
    <template #body>
      <div
        ref="scrollContainer"
        class="h-full overflow-y-auto scrollbar-hidden"
      >
        <section
          v-for="(label, index) in labels"
          :id="sectionIds[index]"
          :key="label"
          :data-section-index="index"
          class="h-full"
        >
          <UContainer class="h-full flex flex-col justify-center gap-4 sm:gap-6 py-8">
            <component :is="sectionComponents[label as keyof typeof sectionComponents]" />
          </UContainer>
        </section>
      </div>
    </template>
  </UDashboardPanel>
</template>
