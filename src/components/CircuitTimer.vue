<template>
    <section>
        <h1>Workout</h1>
        <div @click="toggleMusic()">
            <span v-if="musicOn">
                <AppIconMusicOff />
            </span>
            <span v-else><AppIconMusicOn /></span>
        </div>
        <AppStartButton />
        <SetContainer :workout="initialData" />
        <audio ref="audio">
            <source :src="soundtrack" type="audio/mpeg">
        </audio>
    </section>
</template>

<script>
import SetContainer from './SetContainer';
import AppStartButton from './AppStartButton';
import AppIconMusicOn from './AppIconMusicOn';
import AppIconMusicOff from './AppIconMusicOff';

import data from '../data.json';

export default {
    name: "CircuitTimer",
    components: {
        SetContainer,
        AppStartButton,
        AppIconMusicOn,
        AppIconMusicOff
    },
    props: {
        autoplay: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
		soundtrack: '/assets/soundtrack/XTaKeRuX-Shinigami.mp3',
		musicOn: this.autoplay,
		initialData: data
        }
    },
    methods: {
        toggleMusic() {
            this.musicOn = !this.musicOn;
        },
        playMusic() {
            this.$refs['audio'].play();
        },
        pauseMusic() {
            this.$refs['audio'].pause(); 
        }
    },
    watch: {
        musicOn: function(value) {
            if (!value) this.pauseMusic();
            if (value) this.playMusic();
        }
    },
    mounted() {
        if (this.autoplay) this.playMusic();
	console.log('%cData: ', 'font-size: 200%;color: green', data);
    }
}
</script>
