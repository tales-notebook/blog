<script setup lang="ts">

const route = useRoute()

const locale = computed(() => route.query.locale as string || 'en-US')
const slug = computed(() => route.params.slug as string[])

const { data } = await useAsyncData(route.fullPath, async () => {
    const [localDoc] = await queryContent('/blog/' + slug.value.join('/'))
      .where({
        _locale: locale.value,
      })
      .find()

    if (localDoc) return localDoc

    const [enDoc] = await queryContent('/blog/' + slug.value.join('/'))
      .where({
        _locale: 'en-US',
      })
      .find()

    return enDoc
})

useContentHead(data.value!)

</script>
<template>
  <main v-if="data" class="blog-doc">
    <ContentRenderer :value="data" />
  </main>
</template>

<style lang="scss">
.blog-doc {
  @apply mt-10;
  
  h1, h2, h3, h4, h5, h6 {
    @apply font-bold;
  }

  h1 {
    @apply text-2xl mb-4;
  }

  h2 {
    @apply text-xl my-4;
  }

  p {
    @apply mb-2;

    a {
      @apply text-zinc-500 underline;
    }
  }

  blockquote {
    @apply bg-zinc-100 p-4 my-4 block rounded border-l-4 border-zinc-500;

    p {
      @apply mb-0;
    }
  }

}
</style>
