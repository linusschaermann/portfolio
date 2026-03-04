<script setup>
import { Analytics } from '@vercel/analytics/nuxt'

useHead({
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})

const title = 'Linus Schärmann'
const description = 'Co-Founder of ManagrSphere | Front-End Developer | Business Informatics Student'

useSeoMeta({
  title,
  description,
  ogTitle: title,
  ogDescription: description,
  ogImage: '/Portrait.jpg',
  twitterImage: '/Portrait.jpg',
  twitterCard: 'summary_large_image'
})
</script>

<template>
  <UApp>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <Analytics />
  </UApp>
</template>
