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
            class="circle-chart" 
            viewbox="0 0 180 180" 
            width="180" 
            height="180">
            <circle 
                class="circle-chart__background" 
                stroke="#efefef" 
                stroke-width="10" 
                fill="none" 
                cx="90" 
                cy="90" 
                r="85" />
     
            <circle 
                class="circle-chart__circle circle-chart__circle-progress" 
                stroke="#00acc1" 
                stroke-width="10" 
                fill="none" 
                cx="90" 
                cy="90" 
                r="85" 
                :stroke-dasharray="dashArray" 
                stroke-linecap="round"
                ref="progress" />

            <g class="circle-chart__info">
                <text 
                    class="circle-chart__percent " 
                    x="90" 
                    y="110" 
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
        title: {
            type: String
        },
        paused: {
            type: Boolean,
            default: false
        },
        isLayDown: {
            type: Boolean
        }
    },
    data() {
        return {
            countdown: this.time,
            isPaused: this.paused,
            finished: false,
            oneQuarterTime: this.time * .25,
            oneQuarterDash: ((534 / 100) * .25) * 100 >>> 0,
            dashArrayValue: 0,
            progressCount: 1
        }
    },
    watch: {
        isPaused: function (value) {
            if (value) {
                this.removeCount();
            }
        },
        countdown: function(value, oldValue) {

            if (oldValue === (this.time - (this.progressCount * this.oneQuarterTime) >>> 0)) {
                this.dashArrayValue = this.oneQuarterDash * this.progressCount;
                this.progressCount += 1;
                this.$refs.progress.style.animation = 'none';
                this.$refs.progress.offsetWidth;
                this.$refs.progress.style.animation = 'circle-chart-fill 2s reverse';
                this.$refs.progress.classList.add('circle-chart__circle-progress');
                console.log(this.$refs.progress.classList);
            }
            this.$refs.progress.classList.remove('circle-chart__circle-progress');

            if (oldValue <= 6) {
                if (value === 0) {
                    this.removeCount();
                    this.finished = true;
                    this.$emit('finish');
                } else {
                    this.speak(value);
                }
            }
        }
    },
    computed: {
        currentColor() {
            return this.countdown <= 5 ? 'red' : 'yellow';
        },
        countStatus() {
            return this.isPaused ? "resume" : "pause";
        },
        dashArray() {
            return this.dashArrayValue + ", 534";
        }
    },
    methods: {
        startCount() {
            this.intervalId = setInterval(() => {
                this.countdown -= 1;
            }, 1000);
        },
        removeCount() {
            clearInterval(this.intervalId);
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
            const voices = this.synth.getVoices();
            const utterThis = new SpeechSynthesisUtterance(txt);
            utterThis.voice = voices[0];
            utterThis.pitch = 1;
            utterThis.rate = 1;
            this.synth.speak(utterThis);
        },
        restart(el, done) {
            console.log('animation is ', done);
            el.style.animationPlayState = 'running';
        }
    },
    created() {
        if (!this.paused) {
            this.startCount();
        }
        this.synth = window.speechSynthesis;
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

    .circle-chart__circle {
        transform: rotate(-90deg); /* 2, 3 */
        transform-origin: center; /* 4 */
    }
    .circle-chart__circle-progress {
        animation: circle-chart-fill 2s reverse; /* 1 */ 
    }

    .circle-chart__info {
        animation: circle-chart-appear 2s forwards;
        opacity: 0;
        transform: translateY(0.3em);
    }

    @keyframes circle-chart-fill {
        to { stroke-dasharray: 0 100; }
    }

    @keyframes circle-chart-appear {
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
