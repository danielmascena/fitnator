<template>
    <section>
        <h1>Set 1</h1>
        <AppCountdown 
            v-if="startWarmup"
            title="Warm-up" 
            :time="20" 
            :hasPreview="true"
            :next='iterator.next().value'
            @finish="startWorkout()" />
        <ListAppRound 
            v-else
            :workout="convertedWorkout"
            :iterator="iterator" />
    </section>
</template>

<script>
import ListAppRound from './ListAppRound';
import AppCountdown from './AppCountdown';

import Round from '../Round';

export default {
    name: 'SetContainer',
    components: {
        ListAppRound,
        AppCountdown,
    },
    props: {
        workout: {
            type: Array
        }
    },
    data() {
        return {
            iterator: this.workout.values(),
            startWarmup: true,
        }
    },
    computed: {
        convertedWorkout() {
            return this.workout.map(w => Reflect.constructor.call(Round, {...w}));
        } 
    },
    methods: {
        startWorkout() {
            this.startWarmup = false;
        }
    }
}
</script>
