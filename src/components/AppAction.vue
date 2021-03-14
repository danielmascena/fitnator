<template>
    <section>
        <h1>Exercise 1</h1>
        <app-countdown :time="time" :paused="isPaused" @pause="pauseTheCountdown()"></app-countdown>
    </section>
</template>

<script>
import AppCountdown from './AppCountdown';

export default {
    name: 'AppAction',
    components: {
        AppCountdown
    },
    props: {
        time: {
            type: Number,
            required: true
        }
    },
    data() {
        return {
            isddPaused: false
        }
    },
    methods: {
        pauseTheCountdown() {
            console.log("Time to pause");
            this.isPaused = !this.isPaused;
        }
    }
}
</script>
