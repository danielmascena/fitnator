<template>
  <ul class="list-of-round">
    <li>
    <AppRound v-for="(work, index) in workout" :key="index" :exercise="work" />
    </li>
  </ul>
</template>

<script>
import AppRound from './AppRound';

import Round from '../Round';

export default {
  name: 'ListAppRound',
  components: {
	AppRound
  },
  props: {
    workout: {
      type: Array,
      validator: function(items){
        return items.every(item => item instanceof Round);
      }
    }
  }
};
</script>

<style>
.list-of-round {
  list-style: none;
}
</style>
