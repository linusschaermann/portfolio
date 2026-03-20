<template>
  <UPageHero
    title="Linus Schärmann."
    description="Co-Founder of ManagrSphere | Front-End Developer | Business Informatics Student"
    orientation="horizontal"
  >
    <template #headline>
      <div class="flex justify-start items-center">
        <span class="relative flex size-2">
          <span
            class="absolute inline-flex size-full rounded-full opacity-75 bg-success animate-ping"
          />
          <span
            class="relative inline-flex size-2 scale-90 rounded-full bg-success"
          />
        </span>
        <span class="ml-2 text-sm text-muted">Open for work</span>
      </div>
    </template>
    <img
      src="/Portrait.jpg"
      alt="Portrait"
      class="rounded-lg shadow-2xl ring ring-muted"
    >
  </UPageHero>
</template>
