<script lang="ts" setup>
import PlayIcon_ from '~~/assets/svg/play_icon.svg'
import { useDoubleClick } from '~~/composables/useDoubleClick'
import { useTimeLineStore } from '~~/store/timeline'
import { useVideoPauseViewPort } from '~~/composables/useVideoPauseViewPort'
import { usePercentVideo } from '~~/composables/usePercentVideo'
import { gsap } from 'gsap'

interface IProps {
  video: any
  idPost: string
}
const props = defineProps<IProps>()

const videoRef = ref(null)
const progressBarRef = ref<HTMLDivElement>(null)
const isVideoPlay = ref(false)
const timelineStore = useTimeLineStore()

const isVideoMuted = ref(true)
const toggleLike = () => {
  timelineStore.setToggleLikePost(props.idPost)
}

onMounted(() => {
  videoRef.value.addEventListener('timeupdate', updateTime)
})

const updateTime = () => {
  isVideoPlay.value = !videoRef.value.paused
  isVideoMuted.value = videoRef.value.muted
}

const play = () => {
  const allVideo = document.querySelectorAll('video')
  allVideo.forEach((video) => {
    video.pause()
  })
  videoRef.value.play()
}
const togglePlay = () => {
  if (videoRef.value.paused) {
    play()
  } else {
    videoRef.value.pause()
  }
}

useDoubleClick(videoRef, togglePlay, toggleLike)
useVideoPauseViewPort(videoRef)
const { percent } = usePercentVideo(videoRef)

watch(percent, () => {
  const { clientWidth: widthParent } = progressBarRef.value.parentElement
  Object.assign(progressBarRef.value, {
    width: `${percent.value * widthParent}px`,
  })
})
</script>

<template>
  <div class="group relative min-w-full overflow-hidden" :key="video.id">
    <video :src="video.src" ref="videoRef" loop class="min-w-full" />
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2">
      <div
        ref="bigPlayIcon"
        :class="`absolute top-1/2 left-1/2 h-[90px] w-[90px] origin-center -translate-x-1/2 -translate-y-1/2  ${
          isVideoPlay ? 'scale-0 opacity-0' : ' scale-100 opacity-100'
        } opacity-0 duration-500`"
      >
        <PlayIcon_ @click="play" />
      </div>
    </div>
    <div
      class="absolute bottom-[5px] left-1/2 h-[3px] w-[150px] -translate-x-1/2 bg-white opacity-0 duration-500 group-hover:opacity-100"
    >
      <div ref="progressBarRef" class="h-full w-0 bg-blue-500"></div>
    </div>
  </div>
</template>
