<script lang="ts" setup>
import { Elastic, gsap } from 'gsap'
import LikeIcon_ from '~~/assets/svg/heart_break_icon.svg'

const likeRef = ref<HTMLDivElement>(null)

onMounted(() => {
  const tl = gsap.timeline()

  tl.to(likeRef.value, {
    scale: 5,
    duration: 0.5,
    ease: Elastic.easeOut.config(1, 0.3),
  })
  tl.to(likeRef.value, {
    display: 'none',
    duration: 0.3,
  })
})
</script>

<template>
  <div
    ref="likeRef"
    class="absolute top-[50%] left-[50%] -translate-x-1/2 -translate-y-1/2 scale-125"
  >
    <LikeIcon_ />
  </div>
</template>
