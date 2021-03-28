<template>
    <article>
        <h1>{{exercise.name}}</h1>
        <section v-for="t in exercise.times" :key="t">
            <h2>Round {{t}}</h2>
            <AppCountdown v-if="workStarted" :time="exercise.work" :paused="actionIsPaused" @pause="pauseTheCountdown()" @terminated="switchCounts()" />
            <AppCountdown v-else :time="exercise.rest" :paused="intervalIsPaused" />
        </section>
    </article>
</template>

<script>
import AppCountdown from './AppCountdown';

import Round from '../Round';

export default {
    name: 'RoundTimer',
    components: {
        AppCountdown
    },
    props: {
        exercise: {
            type: Round,
            required: true
        },
    },
    data() {
        return {
		workStarted: true,
            actionIsPaused: false,
            intervalIsPaused: true
        }
    },
    methods: {
        pauseTheCountdown() {
            console.log("Time to pause");
            this.actionIsPaused = !this.actionIsPaused;
        },
	switchCounts(){
		this.workStarted= false;
	}
    }
}
</script>
