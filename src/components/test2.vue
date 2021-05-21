<template>
	<div style="height: 100%">
		<div class="flex-wrap">
			<div class="block">1</div>
			<div class="block">2</div>
			<div class="block">3</div>
		</div>
	</div>
</template>
<style>
.block {
	width: 100%;
	min-height: 100px;
	margin: 10px;
	background: red;
}
.flex-wrap {
	display: flex;
	flex-direction: column;
	height: 100%;
}
.flex-wrap > div:last-child {
	flex: 1;
}
body {
	height: 1000px;
	width: 100%;
	margin: 0;
}
</style>
