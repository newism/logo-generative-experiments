<template>
  <div>
    <svg width="80" viewBox="0 0 400 320" fill="none" xmlns="http://www.w3.org/2000/svg" style="margin: 40px;">
      <path d="M0 320V0L280 320H0Z" fill="#800075" />
      <path d="M280 320L400 1V320H280Z" fill="#FF0099" fill-opacity="0.4" />
      <path d="M0 320V0L280 320H0Z" fill="#FF0099" fill-opacity="0.4" />
      <path d="M120 0L400 320H0L120 0Z" fill="#FF0099" fill-opacity="0.6" />
    </svg>
    <div class="grid" style="--column-width: 200px">
        <Shape v-for="index in 4" :key="`shape-${index}`"></Shape>
    </div>
    <div class="grid" style="--column-width: 200px">
        <Shape v-for="index in 4" :key="`shape-${index}`" inner-stroke-color="rgba(255,255,255,.8)"></Shape>
    </div>
    <div class="grid" style="--column-width: 200px; background: white;">
        <Shape v-for="index in 4" :key="`shape-${index}`" hull-stroke-color="black" inner-stroke-color="black" point-stroke-color="black" :triangle-colours="[]"></Shape>
    </div>
    <div class="grid">
      <Shape :height="600" :width="800" :point-count="5"></Shape>
    </div>
  </div>
</template>

<script>
import Shape from './Shape';

export default {
    name: 'app',
    components: { Shape }
};
</script>

<style>
body {
  background: #40033B;
  padding: 0;
  margin: 0;
}

.grid {
  padding: 40px;
  margin: 40px 0;
  display:grid;
  grid-gap: 40px;
  grid-template-columns: repeat(auto-fit, var(--column-width))
}

</style>
