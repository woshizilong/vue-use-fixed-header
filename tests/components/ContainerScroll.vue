<script setup lang="ts">
import { onBeforeMount, ref, type CSSProperties } from 'vue'
import { useFixedHeader } from '../../src'

const props = defineProps<{
   enterDelta?: number
   leaveDelta?: number
   enterStyles?: CSSProperties
   leaveStyles?: CSSProperties
   simulateInstantRestoration?: boolean
}>()

const containerRef = ref<HTMLElement | null>(null)
const headerRef = ref<HTMLElement | null>(null)

onBeforeMount(() => {
   if (!props.simulateInstantRestoration) return

   window.requestAnimationFrame(() => {
      if (!containerRef.value) throw new Error('containerRef is null')

      containerRef.value.scroll(0, window.innerHeight / 3)
   })
})

useFixedHeader(headerRef, {
   root: containerRef,
   enterDelta: props.enterDelta,
   leaveDelta: props.leaveDelta,
   enterStyles: props.enterStyles,
   leaveStyles: props.leaveStyles,
})
</script>

<template>
   <div class="Wrapper">
      <div class="Scroller" ref="containerRef">
         <div class="Container">
            <header class="Header" ref="headerRef" />
         </div>
      </div>
   </div>
</template>

<style scoped>
.Wrapper {
   height: 100vh;
   height: 100svh;
   background-color: red;
   display: flex;
   justify-content: center;
   align-items: center;
}

.Scroller {
   width: 600px;
   height: 600px;
   overflow-y: scroll;
   overflow-x: hidden;
   background: white;
   resize: horizontal;

   container-type: inline-size;
}

.Container {
   height: 2000px;
}

.Header {
   width: 100%;
   top: 0;
   left: 0;
   height: 80px;
   background: green;
   position: sticky;
}

/** Used in resize.cy.ts */
@container (max-width: 475px) {
   .Header {
      position: relative;
   }
}

@container (max-width: 320px) {
   .Header {
      display: none;
   }
}
</style>
