<template>
    <article>
        <h1>{{exercise.name}}</h1>
        <section v-for="(numOfTimes, index) of exercise.times" :key="index">
            <h2>Round {{index + 1}}</h2>
            <app-action :time="exercise.work" :paused="actionIsPaused" @pause="pauseTheCountdown()"></app-action>
            <app-interval :time="exercise.rest" :paused="intervalIsPaused"></app-interval>
        </section>
    </article>
</template>

<script>
import AppAction from './AppAction';
import AppInterval from './AppInterval';

import Round from '../Round';

export default {
    name: 'RoundTimer',
    components: {
        AppAction,
        AppInterval
    },
    props: {
        exercise: {
            type: Round,
            required: true
        },
        repeat: {
            type: Number,
            default: 1
        }
    },
    data() {
        return {
            actionIsPaused: false,
            intervalIsPaused: true
        }
    },
    computed: {
        repeatedTimes() {
            return new Array(this.repeat);
        }
    },
    methods: {
        pauseTheCountdown() {
            console.log("Time to pause");
            this.actionIsPaused = !this.actionIsPaused;
        }
    }
}
</script>
