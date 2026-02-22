<template>
  <transition name="splash-fade">
    <div v-if="isVisible" class="splash-screen">
      <div class="animation-container">
        <div class="logo-wrapper" :class="{ animate: isAnimating }">
          <div class="logo-main">
            <svg viewBox="0 0 100 120" class="svg-a">
              <defs>
                <linearGradient id="blueGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" stop-color="#ffffff" />
                  <stop offset="100%" stop-color="#3b82f6" />
                </linearGradient>

                <filter id="glow">
                  <feGaussianBlur stdDeviation="3" result="blur"/>
                  <feMerge>
                    <feMergeNode in="blur"/>
                    <feMergeNode in="SourceGraphic"/>
                  </feMerge>
                </filter>
              </defs>

              <path class="stroke leg left" d="M50 10 L20 110" />

              <path class="stroke leg right" d="M50 10 L80 110" />

              <path class="stroke cross" d="M26 54 L74 54" />
            </svg>

            <div class="logo-rest">nimeFlix</div>
          </div>
        </div>

        <div class="light-sweep"></div>
      </div>
    </div>
  </transition>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const isVisible = ref(true)
const isAnimating = ref(false)

const emit = defineEmits(['finished'])

onMounted(() => {
  setTimeout(() => {
    isAnimating.value = true
  }, 100)

  setTimeout(() => {
    isVisible.value = false
    emit('finished')
  }, 5500)
})
</script>

<style scoped>
.splash-screen {
  position: fixed;
  inset: 0;
  background-color: #050505;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  overflow: hidden;
}

.logo-main {
  display: flex;
  align-items: baseline;
  transform: skewX(-5deg);
}

.svg-a {
  width: 100px;
  height: 120px;
  fill: none;
  stroke: url(#blueGrad);
  stroke-width: 16;
  stroke-linecap: round;
  stroke-linejoin: round;
  filter: url(#glow);
}

.leg {
  stroke-dasharray: 140;
  stroke-dashoffset: 140;
}

.crossbar {
  stroke-width: 14;
  stroke-dasharray: 40;
  stroke-dashoffset: 40;
}

.animate .left {
  animation: draw 0.7s 0.2s ease-out forwards;
}

.animate .right {
  animation: draw 0.7s 0.8s ease-out forwards;
}

.animate .crossbar {
  animation: draw 0.4s 1.4s ease-out forwards;
}

@keyframes draw {
  to {
    stroke-dashoffset: 0;
  }
}

.logo-rest {
  font-family: 'Inter', system-ui, sans-serif;
  font-weight: 900;
  font-size: 100px;
  font-style: italic;
  letter-spacing: -6px;
  margin-left: -6px;
  opacity: 0;
  transform: translateX(-40px);
  filter: blur(15px);
  background: linear-gradient(to right, #ffffff, #3b82f6);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.animate .logo-rest {
  animation: text-reveal 1s 2.2s ease forwards;
}

@keyframes text-reveal {
  to {
    opacity: 1;
    transform: translateX(0);
    filter: blur(0);
  }
}

.animate .logo-main {
  animation: cinematic-zoom 1.5s 4s cubic-bezier(0.5, 0, 0.75, 0) forwards;
}

@keyframes cinematic-zoom {
  to {
    transform: scale(6) translateZ(1000px);
    opacity: 0;
    filter: blur(20px);
  }
}

.light-sweep {
  position: absolute;
  inset: 0;
  left: -200%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(50, 150, 255, 0.2),
    rgba(255, 255, 255, 0.5),
    rgba(50, 150, 255, 0.2),
    transparent
  );
  transform: skewX(-25deg);
  pointer-events: none;
}

.animate ~ .light-sweep {
  animation: sweep 3s 2.5s ease-in-out forwards;
}

@keyframes sweep {
  from { left: -200%; }
  to { left: 200%; }
}

.splash-fade-leave-active {
  transition: opacity 1.5s ease;
}

.splash-fade-leave-to {
  opacity: 0;
}

@media (max-width: 768px) {
  .svg-a {
    width: 50px;
    height: 60px;
    stroke-width: 18;
  }

  .logo-rest {
    font-size: 50px;
    letter-spacing: -3px;
  }
}
</style>
