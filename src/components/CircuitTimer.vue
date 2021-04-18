<template>
    <section :class="$style.container">
        <h1>Workout</h1>
        <div @click="toggleMusic()">
            <span v-show="musicOn">
                <AppIconMusicOff />
            </span>
            <span v-show="!musicOn">
                <AppIconMusicOn />
            </span>
            <ul style="padding: 0; list-style-type: none; position: fixed;">
                <li v-for="(track, index) of setList" :key="'XTaKeRuX' + index">
                    <AppMusic v-if="soundtrack === index" :musicPath="track" :shouldPlay="musicOn" />
                </li>
            </ul>
        </div>
        <button @click="switchMusic()">Change Music</button>
        <AppButton />
        <SetContainer :workout="initialData" />
        
    </section>
</template>

<script>
import SetContainer from './SetContainer';
import AppButton from './AppButton';
import AppIconMusicOn from './AppIconMusicOn';
import AppIconMusicOff from './AppIconMusicOff';
import AppMusic from './AppMusic';

import data from '../data.json';


export default {
    name: "CircuitTimer",
    components: {
        SetContainer,
        AppButton,
        AppIconMusicOn,
        AppIconMusicOff,
        AppMusic
    },
    props: {
        autoplay: {
            type: Boolean,
            default: false
        }
    },
    data() {
        const pathToMusic = '/assets/soundtrack/';

        return {            
            setList : [
                pathToMusic + '/XTaKeRuX-Shinigami.mp3',
                pathToMusic + '/XTaKeRuX-White_Crow.mp3',    
                pathToMusic + '/XTaKeRuX-Beast_but_not_Least.mp3',
            ],
            soundtrack: 0,
            musicOn: this.autoplay,
            initialData: data
        }
    },
    methods: {
        toggleMusic() {
            this.musicOn = !this.musicOn;
        },
        switchMusic() {
            let next = this.soundtrack + 1;
            if (next >= this.setList.length) {
                next = 0;
            }
            this.soundtrack = next;
        }
    },
}
</script>

<style module>
.container :global {
    --ion-text-color: yellow;
    background-color: #000;
    height: 100%;
}

</style>
