<script lang="ts" setup>

</script>

<template>
    <div class="text-zinc-700">
        <div class="py-2 text-lg flex gap-x-2 justify-center uppercase font-bold border-b">
            <nuxt-link to="/">Home</nuxt-link>
            <nuxt-link to="/blog">Blog</nuxt-link>
        </div>

        <div class="container mx-auto px-4">
            <slot />

        </div>
    </div>
</template>
