<script setup lang="ts">
import { ref, watch } from 'vue';
import type { Ref } from 'vue';

import Button from '../components/Button.vue'

const _NAVITEMS:string[] = ['Nexus', 'Vault', 'Prologue', 'About', 'Contact'];
const _AUDIOBARS:number[] = [1,2,3,4]

const navContainerRef:Ref<HTMLDivElement|null> = ref(null);
const audioElementRef:Ref<HTMLAudioElement|null> = ref(null);
const isAudioPlaying:Ref<boolean> = ref(false);
const isIndicatorActive:Ref<boolean> = ref(false);

const toggleAudioIndicator = ():void => {
    isAudioPlaying.value = !isAudioPlaying.value
    isIndicatorActive.value = !isIndicatorActive.value
}

watch(() => isAudioPlaying.value, (isPlaying) => {
    if(isPlaying) {
        audioElementRef.value?.play();
    }
    else {
        audioElementRef.value?.pause()
    }
})

</script>

<template>
    <div ref="navContainerRef" class="fixed inset-x-0 top-4 z-50 h-16 border-none transition-all duration-700 sm:inset-x-6">
        <header class="absolute top-1/2 w-full -translate-y-1/2">
            <nav class="flex size-full items-center justify-between p-4">
                <div class="flex items-center gap-7">
                    <img src="/img/logo.png" alt="logo" class="w-10" />
                    <Button :id="'product-button'" :title="'Products'" :right-icon="'mdi:near-me'" :container-class="'bg-blue-50 md:flex hidden items-center justify-center gap-1'" />
                </div>
                <div class="flex h-full items-center">
                    <div class="hidden md:block">
                        <a v-for="(item, itemIdx) in _NAVITEMS" :key="`${item}-${itemIdx}`" class="nav-hover-btn" :href="`#${item.toLowerCase()}`">{{ item }}</a>
                    </div>
                    <button class="ml-10 flex items-center space-x-0.5" @click="toggleAudioIndicator">
                        <audio ref="audioElementRef" class="hidden" src="/audio/loop.mp3" loop />
                        <div v-for="(item, itemIdx) in _AUDIOBARS" :key="`${item}-${itemIdx}`" :class="`indicator-line ${isIndicatorActive ? 'active' : ''}`" :style="`animationDelay:${item * 0.1}s`" />
                    </button>
                </div>
            </nav>
        </header>
    </div>
</template>

<style scoped>
</style>