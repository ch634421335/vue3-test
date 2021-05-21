<template>
	<div>test</div>
	<div>reactive{{ a }}</div>
	<div>传参{{ trans }}</div>
	<div>ref{{ testword2 }}</div>
	<button @click="transFather">testbutton</button>
	<button @click="testword2.b++">plus</button>
</template>
<script setup>
import {
	defineProps,
	reactive,
	defineEmit,
	useContext,
	ref,
	toRefs,
} from "vue";
const testword = reactive({ a: { c: 3 } });
const testword2 = ref({ b: 2 });
defineProps({
	trans: {
		type: String,
		required: false,
		default: "456",
	},
});
function transFather() {
	testword.a.c++;
	testword2.value.b++;
	emit("change", "789");
}
const stateAsRefs = toRefs(testword);
const emit = defineEmit(["change", "delete"]);
const { slots, attrs } = useContext();
console.log(attrs);
</script>
