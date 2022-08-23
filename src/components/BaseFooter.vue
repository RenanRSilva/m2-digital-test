<template>
	<div>
		<div class="gray-line"></div>
		<div class="footer-container">
			<p>
				<strong class="installation-fee">Taxa de instalação</strong>
			</p>
			<p>
				<strong class="installation-fee">Gratis</strong>
			</p>
		</div>
		<div class="footer-container">
			<h3>Total</h3>
			<h3>
				{{
					this.totalPrice.toLocaleString('pt-br', {
						style: 'currency',
						currency: 'BRL'
					})
				}}/Mês
			</h3>
		</div>
		<div class="button-container">
			<button class="continue-button">Continuar</button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'BaseFooter',
	props: {
		totalPrice: Number
	},
	watch: {
		totalPrice() {}
	}
}
</script>

<style scoped>
h3 {
	color: #9247ae;
	margin-top: 10px;
}

p {
	margin-bottom: 0px;
}

.footer-container {
	width: 100%;
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.continue-button {
	padding: 15px 100px 15px 100px;
	background-color: #c7c7c7;
	border: none;
	border-radius: 5px;
	color: white;
	font-size: 1em;
	margin-bottom: 20px;
}

.continue-button:hover {
	background-color: #7e7e7e;
}

.gray-line {
	width: 100%;
	height: 1px;
	background-color: #a8a4a0;
	margin-bottom: 1.6em;
	margin-top: 1.6em;
}

.installation-fee {
	color: #a8a4a0;
}

@media (max-width: 576px) {
	.card-container {
		align-items: center;
		flex-direction: column;
	}
	.continue-button {
		padding: 10px 50px 10px 50px;
		background-color: #c7c7c7;
		font-size: 1em;
		margin-left: 10px;
	}
}
</style>
