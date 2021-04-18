<template>
    <div class="app-btn-centralizer">
        <div class="app-btn-container">
            <button class="app-btn">Start</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "AppButton",
}
</script>

<style scoped>
.app-btn-centralizer {
    display: flex;
    justify-content: center
}
.app-btn-container {
    box-shadow: 0 0 4px var(--ion-text-color);
    width: 100px;
    height: 100px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
}
.app-btn {
    background: none;
    outline: none;
    color: var(--ion-text-color);
    font-size: 150%;
}
</style>
