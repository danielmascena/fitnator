<template>
    <article v-if="display">
        <h1>{{exercise.name}}</h1>
        <section 
            v-for="round in exercise.times" 
            v-show="isCurrentRound(round)"
            :key="round">
                <h2>Round {{round}}</h2>
                <AppCountdown 
                    v-if="isActionCounting" 
                    title="Work"
                    :time="exercise.work" 
                    :paused="!isCurrentRound(round)" 
                    @finish="actionFinished()" 
                />
                <AppCountdown 
                    v-else 
                    title="Rest"
                    :time="exercise.rest" 
                    :paused="!isCurrentRound(round)" 
                    @finish="restFinished()" 
                    @ending="showNext()"
                />
                <AppPreview v-show="shouldDisplayPreview" :exercise="next" />
        </section>
    </article>
</template>

<script>
import AppCountdown from './AppCountdown';
import AppPreview from './AppPreview';

import Round from '../Round';

export default {
    name: 'RoundTimer',
    components: {
        AppCountdown,
        AppPreview,
    },
    props: {
        exercise: {
            type: Round,
            required: true,
        },
        display: Boolean,
        generator: {
            type: Object,
            validator: function (value) {
                return Object.prototype.toString.call(value) === '[object Array Iterator]';
            }
        },
    },
    data() {
        return {
            isActionCounting: true,
            position: 1,
            shouldDisplayPreview: false,
            next: {}
        }
    },
    watch: {
        position(value) {
            if (value > this.exercise.times) {
                console.log('all round finished');
                this.$emit('finish');
            }
        }
    },
    methods: {
        actionFinished() {
            this.isActionCounting = false;
        },
        restFinished() {
            console.log('%crest finished', 'color: blue; font-size: 150%');
            this.position += 1;
            this.isActionCounting = true;
        },
        isCurrentRound(round) {
            return round === this.position;
        },
        showNext() {
            console.log('showing next');
            this.next = this.generator.next().value;
            this.shouldDisplayPreview = true;
            const timerId = setTimeout(function() {
                this.shouldDisplayPreview = false;
                clearTimeout(timerId);
            }, 5000);
        }
    },
    updated: function () {
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been re-rendered
        });
    }
}
</script>
