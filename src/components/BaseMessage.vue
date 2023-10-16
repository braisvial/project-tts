<template>
    <div>
        <textarea class="text" v-model="text" cols="30" rows="10"></textarea>
        <button @click="speak">Hablar</button>
        <button @click="listVoices">Mostrar voces</button>
        <input type="number" v-model="currentVoice">
        <input type="number" min="0" max="2" step="0.25" v-model="currentPitch">
        <input type="number" min="0" max="2" step="0.25" v-model="currentRate">
        <p>Motor actual: {{ voices[currentVoice]?.name }}</p>
    </div>
</template>

<script>
export default {
    name: "baseMessage",
    data() {
        return {
            text: `Las rosas son rojas, Las violetas son azules, El azúcar es dulce, así eres tú`,
            currentVoice: 10,
            voices: [],
            currentPitch: 1,
            currentRate: 1,
        }
    },
    mounted() {
        speechSynthesis.addEventListener("voiceschanged", () => {
            this.voices = speechSynthesis.getVoices()
        })
    },
    methods: {
        speak() {
            const message = new SpeechSynthesisUtterance(this.text);
            message.voice = this.voices[this.currentVoice];
            message.pitch = this.currentPitch;
            message.rate = this.currentRate;
            speechSynthesis.speak(message);
        },
    }
}
</script>

<style scoped>
.text {
    background-color: transparent;
    color: white;
    border: 1px solid white;
    font-size: 1.5rem;
}
</style>