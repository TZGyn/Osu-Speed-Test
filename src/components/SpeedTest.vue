<template>
	<div class="content">
		<div class="stats">
			<h1>{{ count }}</h1>
			<h1 v-if="start">
				{{ time }} ms / {{ (time / 1000).toFixed(2) }} s
			</h1>
			<h1 v-if="start && end">{{ bpm.toFixed(0) }} bpm</h1>
		</div>
		<button @click="reset()">Reset</button>
	</div>
</template>

<style scoped>
	.stats {
		width: 400px;
		height: 400px;
		background-color: #ffffff10;
	}
</style>

<script setup lang="ts">
	import { ref } from 'vue';

	const count = ref(0);
	const time = ref(0);
	const bpm = ref(0);
	const start = ref(false);
	const end = ref(true);
	var date = Date.now();

	var timer: number;

	const reset = () => {
		count.value = 0;
		time.value = 0;
		bpm.value = 0;
		start.value = false;
		end.value = true;
	};

	window.addEventListener('keydown', (e) => {
		if ((e.key === 'a' || e.key === 's') && count.value < 10) {
			if (count.value === 0 && start.value === false) {
				start.value = true;
				end.value = false;
				date = Date.now();
				timer = setInterval(() => {
					time.value = Date.now() - date;
				}, 10);
			}
			count.value++;
		}
		if (count.value === 10) {
			end.value = true;
			clearInterval(timer);
			bpm.value = (count.value * 15) / (time.value / 1000);
		}
	});
</script>
