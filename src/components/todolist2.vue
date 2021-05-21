<template>
	<div>
		<h1>ToDoList</h1>
		<div class="header">
			<input type="text" v-model="inputValue" />
			<button @click="add">确定</button>
		</div>
		<div class="body">
			<ul>
				<li v-for="item in a" :key="item">{{ item }}</li>
			</ul>
		</div>
	</div>
</template>
<script>
import { ref, toRefs, reactive } from "vue";
import _ from "lodash";
export default {
	setup(props, context) {
		const inputValue = ref("");
		const all = reactive({ a: [] });
		function add() {
			all.a.push(inputValue.value);
			inputValue.value = "";
			// console.log(all.a);
			console.log(_.debounce, "-");
		}
		function text() {
			console.log(123);
		}
		console.log(context);
		context.emit("text");
		return {
			...toRefs(all),
			inputValue,
			add,
		};
	},
};
</script>
