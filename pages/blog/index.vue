<script lang="ts" setup>
import type { ParsedContent } from '@nuxt/content/types';


const locale = 'en-US'

const lang = ['en-US', 'pt-BR']

const selected = ref('en-US')

const documents = ref<ParsedContent[]>([])


const { data } = await useAsyncData('blog', () => {
    return queryContent('/blog').where({ _locale: locale }).find()
})

async function setDocuments(){
    const data = await queryContent('/blog').where({ _locale: selected.value }).find()

    documents.value = data
}

watch(selected, setDocuments, { immediate: true })


</script>

<template>
    <div class="flex flex-col max-w-[50rem] w-full mx-auto gap-y-5 p-4">
        <div class="flex gap-x-5 justify-center">
            <button
                v-for="l in lang" :key="l"
                @click="selected = l"
                :class="{
                    'bg-purple-200': selected === l,
                    'bg-zinc-200': selected !== l
                }"
                class="px-4 py-2 rounded"
            >
                {{ l }}
            </button>
        </div>
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
