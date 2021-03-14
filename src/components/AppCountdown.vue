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
        paused: {
            type: Boolean
        }
    },
    data() {
        return {
            countdown: this.time,
            isPaused: this.paused
        }
    },
    watch: {
        isPaused: function (value) {
            console.log('isPaused',value);
            if (value) {
                this.removeCount();
            }
        },
        countdown: function(value) {
            if (value === 0) {
                this.removeCount();
            }
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
        },
        startCount() {
            this.intervalId = setInterval(() => {
            this.countdown -= 1;
            if (this.countdown === 0) this.$emit('finish');
        }, 1000);
        },
        removeCount() {
            clearInterval(this.intervalId);
        }
    },
    created() {
        this.startCount();
    },
    unmounted() {
        this.removeCount()
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