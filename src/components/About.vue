<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/all';

import AnimatedTitle from './AnimatedTitle.vue';


const initClipAnimation = ():void => {
    const clipAnimation = gsap.timeline({
        scrollTrigger: {
            trigger: '#clip',
            start: 'center center',
            end: '+=800 center', // Indica que la animación durará 800px de desplazamiento después de alcanzar el centro del trigger,
            scrub: 0.5, // Sincroniza la animación con el desplazamiento del scroll, con una suavización de 0.5 segundos
            pin: true, // Indica que el elemento se mantendrá fijo durante la animación
            pinSpacing: true // Asegura que el espacio del elemento se mantenga durante el pinning
        }
    })
    clipAnimation.to('.mask-clip-path', {
        width: '100vw',
        height: '100vh',
        borderRadius: 0
    })
}

onMounted(() => {
    initClipAnimation()
})

onUnmounted(() => {
    ScrollTrigger.getAll().forEach(t => t.kill()); // Se limpian todos los scrollTriggers encontrados
})

</script>

<template>
    <div id="about" class="min-h-dvh min-w-dvw">
        <div class="relative mb-8 mt-36 flex flex-col items-center gap-5">
            <h2 class="font-general text-sm uppercase md:text-[0.625rem]">Welcome to Zentry</h2>
            <AnimatedTitle :title="`Disc<b>o</b>ver the world's<br />l<b>a</b>rgest shared`" :container-class="'mt-5 !text-black !text-center'" />
            <div class="about-subtext">
                <p>The Game of Games begins-your life, now an epic MMORPG</p>
                <p>Zentry unites every player from countless games and platforms</p>
            </div>
        </div>
        <div class="h-dvh w-dvw" id="clip">
            <div class="mask-clip-path about-image">
                <img src="/img/about.webp" alt="background" class="absolute left-0 top-0 size-full object-cover">
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>