<template>
	<div class="content">
		<div class="input">
			<input
				type="number"
				placeholder="number of clicks"
				v-model="clickNum"
				v-on:input="reset"
				v-on:keyup.enter="unfocus($event)"
			/>
			<input
				type="text"
				placeholder="first key"
				maxlength="1"
				v-model="firstKey"
				v-on:input="reset"
				v-on:keyup.enter="unfocus($event)"
			/>
			<input
				type="text"
				placeholder="second key"
				maxlength="1"
				v-model="secondKey"
				v-on:input="reset"
				v-on:keyup.enter="unfocus($event)"
			/>
		</div>
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
input {
	background-color: #ffffff20;
	border: none;
	font-size: 1.3em;
	padding: 10px;
	margin: 10px auto;
	border-radius: 10px;
	display: block;
}
</style>

<script setup lang="ts">
import { ref } from 'vue';

const clickNum = ref(10);
const firstKey = ref('x');
const secondKey = ref('z');
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
	clearInterval(timer);
};

const unfocus = (event) => {
	reset();
	event.target.blur();
};

window.addEventListener('keydown', (e) => {
	if (
		(e.key === firstKey.value || e.key === secondKey.value) &&
		count.value < clickNum.value
	) {
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
	if (count.value === clickNum.value) {
		end.value = true;
		clearInterval(timer);
		bpm.value = (count.value * 15) / (time.value / 1000);
	}
});
</script>
