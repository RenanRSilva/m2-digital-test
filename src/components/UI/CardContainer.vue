<template>
	<ul class="card-container">
		<slot></slot>
	</ul>
</template>

<script>
export default {
	name: 'CardContainer'
}
</script>

<style scoped>
.card-container {
	margin-top: 25px;
	display: flex;
	flex-direction: row;
	justify-content: start;
	align-items: start;
	margin-bottom: 50px;
}

@media (max-width: 576px) {
	.card-container {
		align-items: center;
		flex-direction: column;
		margin-top: 5px;
	}
}
</style>
