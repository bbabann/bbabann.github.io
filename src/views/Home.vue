<template>
  <v-container fluid fill-height>
    <v-img id="sheet" style="position: absolute; right: 0%" height="100vh" width="25%" color="black" :src="require('@/assets/d.png')" />

    <div>
      <v-card flat tile :img="require('@/assets/8.png')" :style="`position: absolute; right: 25%; top: ${A}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/7.png')" :style="`position: absolute; right: 34.375%; top: ${B}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/6.png')" :style="`position: absolute; right: 43.75%; top: ${C}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/5.png')" :style="`position: absolute; right: 53.125%; top: ${D}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/4.png')" :style="`position: absolute; right: 62.5%; top: ${E}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/3.png')" :style="`position: absolute; right: 71.875%; top: ${F}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/2.png')" :style="`position: absolute; right: 81.25%; top: ${B}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/1.png')" :style="`position: absolute; right: 90.625%; top: ${C}%`" height="100vh" width="9.375%" />
    </div>

    <div>
      <v-card flat tile :img="require('@/assets/8.png')" :style="`position: absolute; right: 25%; top: ${AX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/7.png')" :style="`position: absolute; right: 34.375%; top: ${BX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/6.png')" :style="`position: absolute; right: 43.75%; top: ${CX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/5.png')" :style="`position: absolute; right: 53.125%; top: ${DX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/4.png')" :style="`position: absolute; right: 62.5%; top: ${EX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/3.png')" :style="`position: absolute; right: 71.875%; top: ${FX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/2.png')" :style="`position: absolute; right: 81.25%; top: ${BX}%`" height="100vh" width="9.375%" />
      <v-card flat tile :img="require('@/assets/1.png')" :style="`position: absolute; right: 90.625%; top: ${CX}%`" height="100vh" width="9.375%" />
    </div>
  </v-container>
</template>

<script>
import { computed, defineComponent, onMounted, reactive, toRefs } from '@vue/composition-api'

export default defineComponent({
  name: 'Home',

  setup(_, { root }) {
    const state = reactive({
      X: 0,
      A: computed(() => (state.X * 2) % 100),
      B: computed(() => state.X),
      C: computed(() => (state.X * 3) % 100),
      D: computed(() => (state.X * 6) % 100),
      E: computed(() => (state.X * 3) % 100),
      F: computed(() => (state.X * 2) % 100),
      AX: computed(() => state.A - 100),
      BX: computed(() => state.B - 100),
      CX: computed(() => state.C - 100),
      DX: computed(() => state.D - 100),
      EX: computed(() => state.E - 100),
      FX: computed(() => state.F - 100),
      x: 0,
      y: 0,
      mouse: false,
      auto: true,
    })

    const drag = (offsetX, offsetY) => {
      state.auto = false

      clearTimeout()

      setTimeout(() => {
        state.auto = true
      }, 30000)

      if (offsetX > 0 || offsetY > 0) offsetX = 1
      else if (offsetX < 0 || offsetY < 0) offsetX = -1

      offset('X', (offsetX * 2) / 10)
    }

    const offset = (k, v) => {
      if (state[k] - v < 0) state[k] = 100 - v
      else if (state[k] - v > 100) state[k] = 0 - v
      else state[k] -= v
    }

    onMounted(() => {
      document.getElementById('sheet').addEventListener('mousemove', (e) => {
        if (state.mouse && (state.x - e.offsetX || state.y - e.offsetY)) drag(state.x - e.offsetX, state.y - e.offsetY)
        state.x = e.offsetX
        state.y = e.offsetY
      })

      document.getElementById('sheet').addEventListener('mousedown', (e) => {
        state.x = e.offsetX
        state.y = e.offsetY
        state.mouse = true
      })

      document.addEventListener('mouseup', () => {
        state.mouse = false
      })

      if (root.$route.params.p) {
        setInterval(() => {
          if (state.auto) offset('X', Number(root.$route.params.p) / 100)
        }, 10)
      }
    })

    return {
      ...toRefs(state),
    }
  },
})
</script>

<style>
body {
  overflow: hidden;
  touch-action: none;
}
</style>
