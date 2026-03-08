<script setup lang="ts">
import { ref, computed, watch, onMounted, onUnmounted } from 'vue';
import type { Ref, ComputedRef } from 'vue';
import gsap from 'gsap';

import Button from './Button.vue';


const _TOTALVIDEOS:number = 3;
let gsapVideoCenterCtx:gsap.Context | null = null

const currentIndex:Ref<number> = ref(1);
const hasClicked:Ref<boolean> = ref(false);
const isLoading:Ref<boolean> = ref(true);
const loadedVideos:Ref<number> = ref(0);

const nextVideoRef:Ref<any> = ref(null);

const upcomingVideoIndex = (cIndex:number):number => ( cIndex % _TOTALVIDEOS ) + 1;

const getVideoSrc = (index:number):string => `videos/hero-${index}.mp4`;

const handleVideoLoad = ():number => loadedVideos.value = loadedVideos.value + 1

// Variable computada que devuelve el indice del video que sera reproducido en background 
const getVideoSrcPlayable:ComputedRef<number> = computed(() => currentIndex.value === _TOTALVIDEOS - 1 ? 1 : currentIndex.value)

const handleMiniVdClick = ():void => {
    if(nextVideoRef.value) {
        hasClicked.value = true;
        currentIndex.value = upcomingVideoIndex(currentIndex.value);
    }
};

// Inicializacion del contexto de video
const initAnimateVideoCenterCtx = ():void => {
    gsapVideoCenterCtx = gsap.context((self) => { // El contexto almacena un grupo de animaciones de manera que facilita labores como limpieza o kill de instancia
        self.add('onClick', (_:any, video:any) => { // onClick puede ser un nombre arbitrario. No apunta directamente a un evento HTML/Javascript
            gsap.set('#next-video', { visibility: 'visible' })

            gsap.to('#next-video', {
                transformOrigin: 'center center',
                scale: 1,
                width: '100%',
                height: '100%',
                duration: 1,
                ease: 'power1.inOut',
                onStart: () => video?.play()
            })

            gsap.from('#current-video', {
                transformOrigin: 'center center',
                scale: 0,
                duration: 1.5,
                ease: 'power1.inOut'
            })
        })
    }, '#hero-parent');
}

watch(() => currentIndex.value, (_) => {
    if(gsapVideoCenterCtx) {
        gsapVideoCenterCtx.revert() // Reinicia las animaciones agrupadas
        gsapVideoCenterCtx.onClick(_, nextVideoRef.value) // Ejecuta el metodo creado al inicializar la instancia 
    }
})

onMounted(() => {
    if(!gsapVideoCenterCtx) { // Si el contexto de animacion es nulo, iniciarlo
        initAnimateVideoCenterCtx()
    }
})

onUnmounted(() => {
    gsapVideoCenterCtx?.kill() // Limpieza de animaciones al desmontar el componente
})

</script>

<template>
    <div id="hero-parent" class="relative h-dvh w-dvw overflow-x-hidden">
        <div id="video-frame" class="relative z-10 h-dvh w-dvw overflow-hidden rounded-lg bg-blue-75">
            <div>
                <div class="mask-clip-path absolute-center absolute z-50 size-64 cursor-pointer overflow-hidden rounded-lg">
                    <div @click="handleMiniVdClick" class="origin-center scale-50 opacity-0 transition-all duration-500 ease-in hover:scale-100 hover:opacity-100">
                        <video ref="nextVideoRef" :src="getVideoSrc(upcomingVideoIndex(currentIndex))" loop muted id="current-video" class="size-64 origin-center scale-150 object-cover object-center" @loadeddata="handleVideoLoad" ></video>
                    </div>
                </div>
                <video ref="nextVideoRef" :src="getVideoSrc(currentIndex)" loop muted id="next-video" class="absolute-center invisible absolute z-20 size-64 object-cover object-center" @loadeddata="handleVideoLoad"></video>
                <video :src="getVideoSrc(getVideoSrcPlayable)" autoplay loop muted class="absolute left-0 top-0 size-full object-cover object-center" @loadeddata="handleVideoLoad"></video>
            </div>
            <h1 class="special-font hero-heading absolute bottom-5 right-5 z-40 text-blue-75">
                G<b>a</b>ming
            </h1>
            <div class="absolute left-0 top-0 z-40 size-full">
                <div class="mt-24 px-5 sm:px-10">
                    <h1 class="special-font hero-heading text-blue-100">redefi<b>n</b>e</h1>
                    <p class="mb-5 max-w-64 font-robert-regular text-blue-100">Enter the Metagame Layer <br /> Unleash the Play Economy
                    </p>
                    <Button :id="'watch-trailer'" :title="'Watch Trailer'" :left-icon="'mdi:near-me'" :container-class="'bg-yellow-300 flex-center gap-1'" />
                </div>
            </div>
        </div>
        <h1 class="special-font hero-heading absolute bottom-5 right-5 text-black">
            G<b>a</b>ming
        </h1>
    </div>
</template>

<style scoped>

</style>