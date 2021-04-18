<template>
    <audio ref="audio">
        <source type="audio/mpeg" :src="musicPath" />
    </audio>
</template>

<script>
export default {
    name: "AppMusic",
    props: {
        musicPath: String,
        shouldPlay: Boolean
    },
    methods: {
        playMusic() {
            this.$refs['audio'].play();
        },
        pauseMusic() {
            this.$refs['audio'].pause(); 
        },
    },
    watch: {
        shouldPlay(value) {
            if (value) {
                this.playMusic();
            } else {
                this.pauseMusic();
            }
        }
    },
    mounted() {
        if (this.shouldPlay) {
            this.playMusic();
        }
    }
}
</script>