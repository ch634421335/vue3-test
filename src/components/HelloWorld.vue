<template>
	<h1>{{ msg }}</h1>

	<button @click="state.count++">count is: {{ state.count }}</button>
	<span>{{ state.test }}</span>
	<p>
		Edit
		<code>components/HelloWorld.vue</code> to test hot module replacement.
	</p>
	<Test :trans="state.test" @change="change2" />
</template>

<script setup>
import { defineProps, reactive } from "vue";
import Test from "./test.vue";
defineProps({
	msg: String,
});
function change2(date) {
	console.log(date);
}
const state = reactive({ count: 0, test: "123" });
</script>

<style scoped>
a {
	color: #42b983;
}
</style>
