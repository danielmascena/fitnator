<template>
    <article>
        <h1>{{this.exercise}}</h1>
        <section v-for="(numOfTimes, index) of repeatedTimes" :key="index">
            <h2>Round {{index + 1}}</h2>
            <app-action :time="45" :paused="actionIsPaused" @pause="pauseTheCountdown()"></app-action>
            <app-interval :time="15" :paused="intervalIsPaused"></app-interval>
        </section>
    </article>
</template>

<script>
import AppAction from './AppAction';
import AppInterval from './AppInterval';

export default {
    name: 'RoundTimer',
    components: {
        AppAction,
        AppInterval
    },
    props: {
        exercise: {
            type: String,
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
