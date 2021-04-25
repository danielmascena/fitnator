<template>
<!--
    <section 
        class="app-countdown-container" 
        @click="pauseCountdown()">
            <h2>{{title}}</h2>
            <time class="app-countdown" 
                :datetime="countdown" 
                :class="{ending: isEnding}">
                    {{countdown}}
            </time>
            <button 
                @click="toggleCount()" 
                class="action-btn">
                    {{countStatus}}
            </button>
    </section>
    -->
    <section class="app-countdown-container">
        <h2>{{title}}</h2>
        <svg 
            xmlns="http://www.w3.org/2000/svg"
            class="circle-container" 
            :height="radius * 2"
            :width="radius * 2">
            <circle 
                class="circle-background" 
                stroke="#efefef" 
                :stroke-width="stroke" 
                fill="none" 
                :cx="radius"
                :cy="radius"
                :r="normalizedRadius" />
     
            <circle 
                class="circle-progress" 
                stroke="#00acc1" 
                :stroke-width="stroke" 
                fill="none" 
                :cx="radius"
                :cy="radius"
                :r="normalizedRadius" 
                :stroke-dasharray="circumference + ' ' + circumference" 
                :stroke-dashoffset="strokeDashoffset"
                stroke-linecap="round"
                ref="progress" />

            <g class="circle-info">
                <text 
                    class="circle-chart__percent " 
                    :x="radius" 
                    :y="radius" 
                    alignment-baseline="central" 
                    text-anchor="middle" 
                    font-size="54" 
                    :style="{fill: currentColor}">
                        {{countdown}}
                </text>
            </g>
        </svg>
        <button 
            @click="toggleCount()" 
            class="action-btn">
                {{countStatus}}
        </button>
    </section>
</template>

<script>

export default {
    name: "AppCountdown",
    props: {
        time: {
            type: Number,
            required: true
        },
        radius: {
            type: Number,
            default: 60
        },
        stroke: {
            type: Number,
            default: 4
        },
        title: String,
        paused: {
            type: Boolean,
            default: false
        },
        isLayDown: Boolean,
    },
    data() {
        const normalizedRadius = this.radius - this.stroke * 2;
        const circumference = normalizedRadius * 2 * Math.PI;
    
        return {
            countdown: this.time,
            isPaused: this.paused,
            dashArrayValue: 0,
            progress: 0,
            tenPercentage: this.time / 100,
            normalizedRadius,
            circumference
        }
    },
    watch: {
        isPaused (value) {
            if (value) {
                this.removeCount();
            }
        },
        countdown (value) {
            if (value <= 5 && value >= 0) {
                this.speak(value);
            } else if (value === 7) {
                this.$emit('ending');
            }
        }
    },
    computed: {
        strokeDashoffset() {
            return this.circumference - this.progress / 100 * this.circumference;
        },
        currentColor() {
            return this.countdown <= 5 ? 'red' : 'yellow';
        },
        countStatus() {
            return this.isPaused ? "resume" : "pause";
        },
    },
    methods: {
        startCount() {
            this.intervalCountdown = setInterval(() => {
                this.countdown -= 1;
                if (this.countdown === 0) {
                    this.removeCount();
                    this.$emit('finish');
                }
            }
            , 1000);
            this.intervalProgress = setInterval(() => this.progress += 1
            , this.tenPercentage * 1000);
        },
        removeCount() {
            clearInterval(this.intervalCountdown);
            clearInterval(this.intervalProgress);
        },
        toggleCount(){
            if (this.isPaused){
                this.startCount();
                this.$emit('resumed');
            } else {
                this.removeCount();
                this.$emit('paused');
            }
            this.isPaused = !this.isPaused;
        },
        speak(txt) {
            const utterThis = new SpeechSynthesisUtterance(txt);
            utterThis.voice = this.voices[this.speakerIndex];
            utterThis.pitch = 1;
            utterThis.rate = 1.3;
            this.synth.speak(utterThis);
        },
    },
    created() {
        if (!this.paused) {
            this.startCount();
        }
        this.synth = window.speechSynthesis;
        this.voices = this.synth.getVoices();
        this.speakerIndex = this.voices.findIndex(({lang, name}) => name === 'Alex' && lang === "en-US");
    },
    unmounted() {
        this.removeCount()
    }
};
</script>


<style>
    .app-countdown-container {
        display: grid;
        place-items: center;
        height: 300px;
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
    .action-btn {
        text-transform: Capitalize;
    }
    .circle-progress {
        transition: stroke-dashoffset 0.35s;
        transform: rotate(-90deg);
        transform-origin: 50% 50%;
    }
    .circle-info {
        animation: circle-info-appear 2s forwards;
        opacity: 0;
        transform: translateY(0.3em);
    }

    @keyframes circle-info-appear {
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
