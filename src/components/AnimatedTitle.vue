<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import type { Ref } from 'vue'
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/all';

let gsapCtx:gsap.Context | null = null

interface Props {
    title?: string
    containerClass?: string
}

const { title = '', containerClass = '' } = defineProps<Props>();

const containerRef:Ref<HTMLDivElement | null> = ref(null);


const initAnimation = ():void => {
    gsapCtx = gsap.context((self) => {
        self.add('onClick', (_:any):void => {
            const titleAnimation = gsap.timeline({
                scrollTrigger: {
                    trigger: containerRef.value,
                    start: '100 bottom',
                    end: 'center bottom',
                    toggleActions: 'play none none reverse' // Define las acciones de reproducción para la animación (play, pause, resume, reverse)
                }
            });
            titleAnimation.to('.animated-word', {
                opacity: 1,
                transform: 'translate3d(0,0,0) rotateY(0deg) rotateX(0deg)',
                ease: 'power2.inOut',
                stagger: 0.02
            })
        });
    }, containerRef.value!);
    gsapCtx.onClick()
}


onMounted(() => {
    if(!gsapCtx) {
        initAnimation()
    }
})

onUnmounted(() => {
    gsapCtx?.kill() // Limpieza al desmontar componente
    ScrollTrigger.getAll().forEach(t => t.kill()) // Limpieza al desmontar componente
})

</script>


<template>
    <div ref="containerRef" class="animated-title" :class="containerClass">
        <div v-for="(line, indexLine) in title.split('<br />')" :key="`line-${indexLine}`" class="flex-center max-w-dvw flex-wrap gap-2 px-10 md:gap-3">
            <span v-for="(word, indexWord) in line.split(' ')" :key="`line-${indexLine}-word-${indexWord}`" v-html="word" class="animated-word"></span>
        </div>
    </div>
</template>

<style scoped>

</style>