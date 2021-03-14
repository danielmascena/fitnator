<template>
  <ion-icon :icon="volumeHighOutline"></ion-icon>
</template>

<script>
import { volumeHighOutline } from 'ionicons/icons';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Icon',
  setup() {
    return { volumeHighOutline }
  }
});
</script>

<style scoped>
ion-icon {
  color: #FFF;
}
</style>