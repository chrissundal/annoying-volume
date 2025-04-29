<script setup>
import Volumknapp from './components/Volumknapp.vue';
import { ref } from 'vue';

const play = ref(false);
const start = () => {
	play.value = true;
}
</script>

<template>
	<div v-if="play">
		<Volumknapp />
	</div>
	<div v-else>
		<button @click="start">Irriterende volumkontroller</button>
	</div>
</template>

<style scoped>
main {
	text-align: center;
	padding: 1em;
	max-width: 240px;
	margin: 0 auto;
}

h1 {
	color: #ff3e00;
	text-transform: uppercase;
	font-size: 3em;
	font-weight: 100;
}

@media (min-width: 640px) {
	main {
		max-width: none;
	}
}
</style>
