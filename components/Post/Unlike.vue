<script lang="ts" setup>
import { Elastic, gsap } from 'gsap'
import UnlikeIcon_ from '~~/assets/svg/unlike_icon.svg'

const unlikeRef = ref<HTMLDivElement>(null)

onMounted(() => {
  const tl = gsap.timeline()

  tl.to(unlikeRef.value, {
    scale: 5,
    duration: 0.5,
    ease: Elastic.easeOut.config(1, 0.3),
  })
  tl.to(unlikeRef.value, {
    display: 'none',
    duration: 0.3,
  })
})
</script>

<template>
  <div
    ref="unlikeRef"
    class="absolute top-[50%] left-[50%] -translate-x-1/2 -translate-y-1/2 scale-125"
  >
    <UnlikeIcon_ />
  </div>
</template>
