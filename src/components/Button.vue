<script setup lang="ts">
import { Icon } from '@iconify/vue'

interface Props {
    id?: string
    title?: string
    leftIcon?: string
    rightIcon?: string
    containerClass?: string
}

const { id = '', title = '', rightIcon = '', leftIcon = '', containerClass = '' } = defineProps<Props>()


</script>

<template>
    <button :id :class="`group relative z-10 w-fit cursor-pointer overflow-hidden rounded-full bg-violet-50 px-7 py-3 text-black ${containerClass}`">
        <Icon v-if="leftIcon" :icon="leftIcon" width="24" height="24" />
        <span class="relative inline-flex overflow-hidden font-general text-xs uppercase">{{ title }}</span>
        <Icon v-if="rightIcon" :icon="rightIcon" width="24" height="24" />
    </button>
</template>

<style scoped>

</style>