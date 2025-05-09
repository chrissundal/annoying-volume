<script setup>
import {ref, onMounted, watch} from "vue";

let audio = new Audio('axelf.m4a')
	audio.volume = 1.0;
	audio.loop = true;

const isOpen = ref(false);

const volume = ref(audio.volume)
const activeButtonIndex = ref(0);
const counter = ref(0)
const message = ref(null);

const phrases = ref([
	"Get ready for the crazy frog!",
	"Ring ding ding ding dingeringeding!",
	"Wa-pa-pa pa-pa-pa-pow!",
	"Hatee-hatee-hatee-ho!",
	"Bom-bom-bom-bom!",
	"Ba-da-ba-da-ba-da!",
	"Ding,ding",
	"Here we go!"])

watch(volume, (newVolume) => {
	audio.volume = newVolume;
});

	const randomizePosition = () => {
		const currentButton = buttons.value[activeButtonIndex.value];
		currentButton.x = Math.random() * (window.innerWidth - 100);
		currentButton.y = Math.random() * (window.innerHeight - 50);
	};

	onMounted(() => {
		setInterval(randomizePosition, 600);
		setInterval(random_phrase, 1000);
		audio.play();
	})

const buttons = ref([
	{ id: 1, img: "/annoying-volume/1.png", x: Math.random() * (window.innerWidth - 100), y: Math.random() * (window.innerHeight - 50),
	},
	{ id: 2, img: "/annoying-volume/2.png", x: Math.random() * (window.innerWidth - 100), y: Math.random() * (window.innerHeight - 50),
	},
	{ id: 3, img: "/annoying-volume/3.png", x: Math.random() * (window.innerWidth - 100), y: Math.random() * (window.innerHeight - 50),
	},
]);

const sliderPosition = ref({
	position: 'absolute',
	bottom: '-220px',
	left: '50%',
	transform: 'translateX(-50%)'
});


const handleButtonClick = () => {
	if(activeButtonIndex.value < buttons.value.length) {
		activeButtonIndex.value++;
		counter.value++;
		console.log(counter.value)
	}
	else if(counter.value === 3){
		
			activeButtonIndex.value = -1;
			message.value = "Du vant, nå kan du endre volum!";
			isOpen.value = true;
			audio.pause();

			setTimeout(() => {
				isOpen.value = false;
				message.value = "Eller ikke...";
			}, 3000)
			setTimeout(() => {
				message.value = null;
				activeButtonIndex.value = 0;
				audio.play();
				audio.playbackRate = 1.25;
			}, 5000)
	}
		else {
			activeButtonIndex.value = -1;
			isOpen.value = true;
			message.value = "Nå kan du faktisk endre volum!"
		}
};

const randomizeSliderOnHover = () => {
	if (isOpen.value) {
		sliderPosition.value = {
			position: 'absolute',
			bottom: `${Math.random() * 300 - 250}px`,
			left: `${Math.random() * 80 + 10}%`,
			transform: 'translateX(-50%)',
		}
		
			if(volume.value !== 1.0) {
				sliderPosition.value = {
					position: 'fixed',
					bottom: '20px',
					left: '50%',
					transform: 'translateX(-50%)',
					textAlign: 'center',
				};
			}
	
	}
};

const random_phrase = () => {
	return phrases.value[Math.floor(Math.random() * phrases.value.length)];
}


</script>

<template>
	<div>
		<div v-if="message">{{message}}</div>
		<div>{{random_phrase()}}</div>
		<div v-if="activeButtonIndex !== -1">
			
			<button
			        class="floating-button"
			        :style="{top: buttons[activeButtonIndex].y + 'px', left: buttons[activeButtonIndex].x + 'px'}"
			        @click="handleButtonClick(buttons[activeButtonIndex])">
							<img :src="buttons[activeButtonIndex].img"/>
			</button>
		</div>
		<div class="slider-container" @mouseover="randomizeSliderOnHover" :style="sliderPosition">
			<div v-if="isOpen">
				<input class="slider" type="range" min="0" max="1" step="0.01" v-model="volume"/>
			<p> Volum: {{Math.floor(volume*100)}}%</p>
		</div>
			</div>
	</div>
</template>

<style scoped>
.floating-button {
	position: absolute;
	background-color: transparent;
	color: white;
	border: none;
	cursor: pointer;
	padding: 5px;
	border-radius: 5px;
	width: 50px;
	font-size: 10px;
}

.slider-container {
	transition: all 0.3s ease;
}

img{
	height: 150px;
	width: 150px;
	border-radius: 75px;
	animation: spin 2s linear infinite, glow 0.2s infinite;
}

@keyframes spin {
	0% {
		transform: rotate(0deg);
	}
		50%{
			transform: rotate(180deg) scale(1.15);
		}
	75%{
		transform: rotate(-190deg) scale(1.3);
	}
	100% {
		transform: rotate(360deg);
	}
}

@keyframes glow {
	0% { opacity: 1; scale: 1; }
	50% { opacity: 0.5; scale: 1.1;}
	100% { opacity: 1; scale: 1;}
}

.slider {
	width: 300px;
}
</style>