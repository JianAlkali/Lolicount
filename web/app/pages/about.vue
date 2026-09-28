<script setup lang="ts">
// About page: project details card, the live theme showcase (NoticeBoard)
// and the star-plea card featuring the begging mascot. All copy comes
// from the i18n dictionary; the star count is fetched client-side via
// useGitHub (10-min localStorage cache, graceful offline fallback).
const { t } = useI18n()
const { stars, repoUrl, fetchStars, formatStars } = useGitHub()

const issuesUrl = `${repoUrl}/issues`
const contributeUrl = `${repoUrl}/blob/main/docs/contributing-themes.md`

// Static facts for the details grid. Links (feedback/contributing) are
// rendered separately as anchor cards.
const details = computed(() => [
  { label: t('about.techStack'), value: t('about.stackValue') },
  { label: t('about.deployment'), value: t('about.deploymentValue') },
  { label: t('about.license'), value: 'AGPL-3.0' },
])

onMounted(() => {
  fetchStars()
})
</script>

<template>
  <main class="max-w-3xl mx-auto px-4 py-8 font-sans">

    <!-- About header -->
    <section id="about" class="mb-10 scroll-mt-20">
      <h2 class="text-2xl font-semibold mb-4 flex items-center gap-2">
        <img src="/images/lolicount-icon.png" alt="" class="h-7 w-7" />
        {{ t('about.title') }}
      </h2>
      <p class="text-sm text-gray-600">{{ t('about.desc') }}</p>
    </section>

    <!-- Project details -->
    <section class="mb-10">
      <h2 class="text-xl font-semibold mb-3">{{ t('about.detailsTitle') }}</h2>
      <div class="rounded-xl bg-loli-cream p-4 grid sm:grid-cols-2 gap-3">
        <div
          v-for="d in details"
          :key="d.label"
          class="rounded-lg bg-white p-3"
        >
          <p class="text-xs text-gray-400 mb-1">{{ d.label }}</p>
          <p class="text-sm font-medium text-gray-700">{{ d.value }}</p>
        </div>
        <a
          :href="issuesUrl"
          target="_blank"
          rel="noopener"
          class="rounded-lg bg-white p-3 group"
        >
          <p class="text-xs text-gray-400 mb-1">{{ t('about.feedback') }}</p>
          <p class="text-sm font-medium text-loli-pink group-hover:underline">
            GitHub Issues →
          </p>
        </a>
        <a
          :href="contributeUrl"
          target="_blank"
          rel="noopener"
          class="rounded-lg bg-white p-3 group"
        >
          <p class="text-xs text-gray-400 mb-1">{{ t('about.contribute') }}</p>
          <p class="text-sm font-medium text-loli-pink group-hover:underline">
            {{ t('about.contributeLink') }} →
          </p>
        </a>
      </div>
    </section>

    <!-- Star plea: begging mascot + live star count -->
    <section class="mb-12">
      <div class="rounded-2xl bg-loli-cream p-6 sm:p-8 flex flex-col sm:flex-row items-center gap-6">
        <img
          src="/images/plz.jpg"
          :alt="t('about.starAlt')"
          class="w-40 sm:w-52 rounded-2xl shadow-md -rotate-2 shrink-0"
          loading="lazy"
        />
        <div class="flex-1 text-center sm:text-left">
          <h2 class="text-2xl font-bold text-loli-pink mb-2">{{ t('about.starTitle') }}</h2>
          <p class="text-sm text-gray-600 mb-4">{{ t('about.starDesc') }}</p>
          <div class="flex flex-wrap items-center gap-3 justify-center sm:justify-start">
            <a
              :href="repoUrl"
              target="_blank"
              rel="noopener"
              class="inline-flex items-center gap-2 bg-loli-pink text-white text-sm font-semibold px-5 py-2.5 rounded-xl shadow hover:bg-loli-pink/90 transition"
            >
              <span>★</span> {{ t('about.starButton') }}
            </a>
            <span v-if="stars != null" class="text-sm text-gray-500">
              {{ t('about.starCount', { n: formatStars(stars) }) }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <SiteFooter />
    <BackToTop />
  </main>
</template>
