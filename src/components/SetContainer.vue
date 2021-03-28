<template>
    <section>
        <h1>Set 1</h1>
	<ListAppRound :workout="convertedWorkout" />
    </section>
</template>

<script>
import ListAppRound from './ListAppRound';
import Round from '../Round';

export default {
    name: 'SetContainer',
    components: {
	ListAppRound
    },
    props: {
	workout: {
          type: Array
	}
    },
    computed: {
      convertedWorkout() {
        return this.workout.map(w => Reflect.constructor.call(Round, {...w}));
      } 
    }
}
</script>
