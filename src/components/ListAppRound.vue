<template>
    <section 
        class="list-of-round">
            <AppRound v-for="(work, index) in workout" 
                :key="index" 
                :exercise="work" 
                :display="isActive(index)"
                @finish="goToNext()" 
                v-show="isActive(index)"
                :generator="iterator"
            />
    </section>
</template>

<script>
import AppRound from './AppRound';
import Round from '../Round';

export default {
    name: 'ListAppRound',
    components: {
        AppRound
    },
    props: {
        workout: {
            type: Array,
            validator: function(items) {
                return items.every(item => item instanceof Round);
            }
        },
    },
    data() {
        return {
            position: 0,
        }
    },
    methods: {
        goToNext() {
            this.position += 1;
        },
        isActive(index) {
            return index === this.position;
        }
    },
    created() {
            this.iterator = this.workout.values();
    }
};
</script>

