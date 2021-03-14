<template>
    <div class="app-countdown-container" @click="pauseCountdown()">
        <time class="app-countdown" :datetime="countdown" :class="{ending: isEnding}">{{countdown}}</time>
    </div>
</template>

<script>

export default {
    name: "AppCountdown",
    props: {
        time: {
            type: Number,
            required: true
        },
    },
    data() {
        return {
            countdown: this.time
        }
    },
    computed: {
        isEnding() {
            return this.countdown <= 5
        }
    },
    methods: {
        pauseCountdown() {
            this.$emit('pause');
        }
    },
    created() {
        this.intervalId = setInterval(() => {
            this.countdown -= 1;
            if (this.countdown === 0) this.$emit('finish');
        }, 1000);
    },
    unmounted() {
        clearInterval(this.intervalId);
    }
};
</script>


<style scoped>
    .app-countdown-container {
        font-size: 500%;
    }
    .app-countdown {
        background-color: rgba(0,0,0,.1);
        border-radius: 5%;
        box-shadow: 0px 3px 3px rgba(0,0,0,.5);
        padding: 15px;
        cursor: pointer;
        color: whitesmoke;
    }
    .app-countdown:active {
        background-color: green;
    }
    .ending {
        color: red;
    }
</style>