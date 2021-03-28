<template>
    <div class="str-btn-centralizer">
        <div class="str-btn-container">
            <button class="str-btn">Start</button>
        </div>
    </div>
</template>

<script>

export default {
    name: "AppStartButton",

}

</script>


<style scoped>

.str-btn-centralizer {
    display: flex;
    justify-content: center
}

.str-btn-container {
    box-shadow: 0 0 4px var(--ion-text-color);
    width: 100px;
    height: 100px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
}

.str-btn {
    background: none;
    outline: none;
    color: var(--ion-text-color);
    font-size: 150%;
}
</style>
