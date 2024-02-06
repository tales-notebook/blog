<script lang="ts" setup>

const locale = 'en-US'

const { data } = await useAsyncData('blog', () => {
    return queryContent('/blog').where({ _locale: locale }).find()
})

const documents = computed(() => data?.value || [])

</script>

<template>
    <div class="flex flex-col max-w-[50rem] w-full mx-auto mt-10">
        <nuxt-link
            v-for="d in documents" :key="d._path"
            class="border p-4 border-zinc-100 rounded shadow block w-full"
            :to="{
                path: d._path,
                query: { locale: d._locale }
            }"
        >

            <div class="font-bold mb-2">
                {{ d.title }}
            </div>

            <div class="text-sm text-zinc-600">
                {{ d.description }}
            </div>
        </nuxt-link>
    </div>
</template>
