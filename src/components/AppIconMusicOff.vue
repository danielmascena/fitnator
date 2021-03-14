<template>
  <ion-icon :icon="volumeMuteOutline"></ion-icon>
</template>

<script>
import { volumeMuteOutline } from 'ionicons/icons';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Icon',
  setup() {
    return { volumeMuteOutline }
  }
});
</script>

<style scoped>
ion-icon {
  color: #FFF;
}
</style>