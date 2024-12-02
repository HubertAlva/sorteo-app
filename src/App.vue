<script setup lang="ts">
import Header from './components/Header.vue'
import Main from './components/Main.vue'
import Footer from './components/Footer.vue'
import { Fireworks } from '@fireworks-js/vue'
import { ref } from 'vue'
import type { FireworksOptions } from '@fireworks-js/vue'

const fw = ref<InstanceType<typeof Fireworks>>()
const options = ref<FireworksOptions>({ opacity: 0.5 })

async function startFireworks() {
  if (!fw.value) return
  fw.value.start()
  // await new Promise((resolve) => setTimeout(resolve, 1000))
  // await fw.value.waitStop()
}

async function stopFireworks() {
  if (!fw.value) return
  await fw.value.waitStop()
}
</script>

<template>
  <main class="flex items-center justify-center w-full min-h-dvh">
    <article
      class="z-50 w-full max-w-lg px-4 py-8 m-4 space-y-8 border rounded-lg md:px-8 bg-slate-950/75 backdrop-blur-sm"
    >
      <Header />

      <Main @winners-announced="startFireworks" @clear="stopFireworks" />

      <Footer />
    </article>
    <Fireworks
      ref="fw"
      :autostart="false"
      :options="options"
      :style="{
        top: 0,
        left: 0,
        width: '100%',
        height: '100%',
        position: 'fixed',
        background: '#000',
      }"
    />
  </main>
</template>
