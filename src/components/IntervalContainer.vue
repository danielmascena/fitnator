<template>
    <section>
        <h1>Exercise 1</h1>
        <app-countdown :time="20" :paused="isPaused" @pause="pauseTheCountdown()"></app-countdown>
    </section>
</template>

<script>
import AppCountdown from './AppCountdown';

export default {
    name: 'IntervalContainer',
    components: {
        AppCountdown
    },
    data() {
        return {
            isPaused: false
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
