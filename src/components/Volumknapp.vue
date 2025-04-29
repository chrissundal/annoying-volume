<script setup>
import { ref, onMounted } from "vue";

let audio = new Audio('axelf.m4a')
	audio.volume = 1.0;
	audio.loop = true;
	const isOpen = ref(false);
	const buttonX = ref(Math.random() * window.innerWidth);
	const buttonY = ref(Math.random() * window.innerHeight);

	const randomizePosition = () => {
		buttonX.value = Math.random() * (window.innerWidth - 100);
		buttonY.value = Math.random() * (window.innerHeight - 50);
	};

	const adjustVolume = () => {
		isOpen.value = !isOpen.value;
	};

	onMounted(() => {
		setInterval(randomizePosition, 600);
		audio.play();
	})
</script>

<template>
	<div>
		<button class="floating-button" :style="{ top: buttonY + 'px', left: buttonX + 'px' }" @click="adjustVolume">Juster volum</button>
		<div class="slider-container">
			<div v-if="isOpen">
				<input class="slider" type="range" min="0" max="1" step="0.01" v-model="audio.volume"/>
			</div>
			<p>Volum: {{Math.floor(audio.volume*100)}}</p>
		</div>
	</div>
</template>

<style scoped>
.floating-button {
	position: absolute;
	background-color: tomato;
	color: white;
	border: none;
	cursor: pointer;
	padding: 5px;
	border-radius: 5px;
	width: 50px;
	font-size: 10px;
}

.slider-container {
	position: fixed;
	bottom: 20px;
	left: 50%;
	transform: translateX(-50%);
	text-align: center;
}

.slider {
	width: 300px;
}
</style>