<template>
    <article>
        <h1>{{exercise.name}}</h1>
        <section v-for="round in exercise.times" :key="round">
            <div v-show="isCurrentRound(round)">
                <h2>Round {{round}}</h2>
                <AppCountdown v-if="isActionCounting" 
                    :time="exercise.work" 
                    title="Work"
                    :paused="!isCurrentRound(round)" 
                    @pause="pauseTheCountdown()" 
                    @finish="actionFinished()" 
                />
                <AppCountdown v-else 
                    :time="exercise.rest" 
                    title="Rest"
                    :pause="!isCurrentRound(round)" 
                    @finish="restFinished()" 
                />
            </div>
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
            isActionCounting: true,
            position: 1
        }
    },
    watch: {
        position(value) {
            if (value === this.exercise.times) {
                console.log('all round finished');
                this.$emit('finish');
            }
        }
    },
    methods: {
        actionFinished(){
            this.isActionCounting = false;
        },
        restFinished(){
            console.log('%crest finished', 'color: blue; font-size: 150%');
            this.position += 1;
        },
        isCurrentRound(round){
            return round === this.position;
        }
    },
    created() {
        console.log('AppRound: ', this.exercise);
    },
    updated: function () {
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been re-rendered
        });
    }
}
</script>
