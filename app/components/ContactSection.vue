<script setup lang="ts">
import type { ButtonProps } from '@nuxt/ui'

const links = ref<ButtonProps[]>([
  {
    label: 'Mail',
    color: 'neutral',
    trailingIcon: 'i-lucide-mail',
    to: 'mailto:linus.schaermann@gmail.com'

  },
  {
    label: 'LinkedIn',
    color: 'neutral',
    variant: 'subtle',
    trailingIcon: 'i-lucide-arrow-right',
    target: '_blank',
    to: 'https://www.linkedin.com/in/linus-sch%C3%A4rmann-4990372b1/'
  }
])
</script>

<template>
  <UPageCTA
    title="Interested?"
    description="Feel free to reach out if you'd like to learn more about me or my work."
    :links="links"
  />
</template>
