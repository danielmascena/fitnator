<template>
  <article class="next-container">
    <h1>Coming next:</h1>
    <p><span>{{exercise.name}}</span> for <time :datetime="exercise.work">{{exercise.work}}</time></p>
  </article>
</template>

<script>
import Round from '../Round';

export default {
  name: 'AppPreview',
  props: {
    exercise: Round
  }
}
</script>

<style scoped>
.next-container {
  position: fixed;
  bottom: 30px;
  right: 10px;
  z-index: 10;
  color: white;
  border: 2px solid yellow;
}
</style>