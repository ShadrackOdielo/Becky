<script setup>
import { UTooltip } from '#components'
import { onMounted, onUnmounted } from 'vue'

const scrollTop = () => {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}
const visible = ref(false)
const threshold = 100 // Adjust this value to control when the button appears

function onScroll() {
	visible.value = window.scrollY > threshold
}

onMounted(() => {
	window.addEventListener('scroll', onScroll)
})

onBeforeUnmount(() => {
	window.removeEventListener('scroll', onScroll)
})
onMounted(() => {
  const scrollProgress = document.querySelector('.scroll-top__progress')
  // check if scrollProgress is null 

  
  
  const updateProgress = () => {
    if (scrollProgress) {
      const scrollPercentage = (window.scrollY / (document.documentElement.scrollHeight - document.documentElement.clientHeight)) * 100
      const radius = parseFloat(scrollProgress.getAttribute('r'))
      const circumference = 2 * Math.PI * radius
      const offset = circumference * (1 - (scrollPercentage / 100))
      scrollProgress.style.strokeDasharray = `${circumference} ${circumference}`
      scrollProgress.style.strokeDashoffset = offset
    }
  }

  window.addEventListener('scroll', updateProgress, )
  updateProgress()

  onUnmounted(() => window.removeEventListener('scroll', updateProgress))
})
</script>
<template>
  <div v-show="visible" class="scroll-top z-99" aria-label="Scroll to top" @click="scrollTop">
     <UTooltip
      text="Scroll to top"
      placement="left">
    <UIcon  name="i-lucide-chevron-up-solid" class="text-2xl" /></UTooltip>
    <svg class="scroll-top__circle" viewBox="0 0 40 40">
      <!-- Half of the thickness of the circle must be subtracted from the radius, otherwise the graphic will be cut off -->
      <circle class="scroll-top__progress" cx="20" cy="20" r="18"/>
    </svg>
  </div>
</template>
<style scoped>

.scroll-top {
  --size: 40px;
  position: fixed;
  right: 50px;
  bottom: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: var(--size);
  height: var(--size);
  color: var(--color-neutral);
  background-color: var(--bg-secondary);
  border-radius: 50%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  text-decoration: none;
}
.scroll-top__circle {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform: rotate(-90deg);
  pointer-events: none;
  paint-order: stroke;
}
.scroll-top__progress {
  fill: none;
  stroke: currentColor;
  stroke-width: 2; /* Pay attention to the radius */
}
</style>
