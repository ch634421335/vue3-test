<template>
	<div>
		<div>ToDoList</div>
		<div class="header">
			<input type="text" v-model="inputValue" />
			<button @click="add">确定</button>
		</div>
		<div>ToDo</div>
		<div class="body">
			<ul>
				<li v-for="item in all.a" :key="item">
					<input @click="confirm(item)" type="checkbox" />{{ item }}
				</li>
			</ul>
		</div>
		<div>Done</div>
		<div class="foot">
			<ul>
				<li v-for="item in all.b" :key="item">{{ item }}</li>
			</ul>
		</div>
	</div>
</template>
<script setup>
import { ref, reactive } from "vue";
const inputValue = ref("");
const all = reactive({ a: [], b: [] });
function add() {
	all.a.push(inputValue.value);
	inputValue.value = "";
}
function confirm(value) {
	all.b.push(value);
	all.a = all.a.filter((item) => {
		return all.b.indexOf(item) == -1;
	});
}
</script>
