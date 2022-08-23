<template>
	<card-container>
		<li
			@click="updatePrice"
			v-for="product in internetProducts"
			v-bind:key="product.id"
			class="card element"
		>
			<input
				v-if="reloadInput"
				:id="product.id"
				:value="product.id"
				type="radio"
				v-model="selectedInternetId"
			/>
			<h2 class="internet-title">
				{{ product.name }}
			</h2>
			<p>
				<strong>{{
					product.price.toLocaleString('pt-br', {
						style: 'currency',
						currency: 'BRL'
					})
				}}</strong>
			</p>
			<p class="details">- DETALHES</p>
			<button
				class="close-btn"
				v-if="product.id === selectedInternetId"
				@click="removeFromCart"
			>
				X
			</button>
		</li>
	</card-container>
</template>

<script>
import cardContainer from './UI/CardContainer.vue'

export default {
	name: 'InternetCardRow',
	emits: ['update-internet'],
	components: {
		cardContainer
	},
	props: {
		internetProducts: {}
	},
	data() {
		return {
			selectedInternetId: null,
			internetPrice: 0,
			reloadInput: true
		}
	},
	methods: {
		removeFromCart: function () {
			this.selectedInternetId = 0
			this.internetPrice = 0
			this.reloadInput = !this.reloadInput
			this.reloadInput = !this.reloadInput
			this.$emit('update-internet-selected', this.internetPrice)
		}
	},
	watch: {
		selectedInternetId() {
			this.internetPrice = 0
			if (this.selectedInternetId) {
				const selectedInternet = this.internetProducts.filter(
					(product) => this.selectedInternetId === product.id
				)
				const internetObj = JSON.parse(JSON.stringify(selectedInternet))[0]
				this.internetPrice = internetObj.price
				this.$emit('update-internet-selected', this.internetPrice)
			}
		}
	}
}
</script>

<style scoped>
p strong {
	color: black;
}
.internet-title {
	font-size: 1.4em;
	font-weight: bolder;
	margin-top: 5px;
}

.details {
	text-align: right;
	margin-bottom: 2px;
	margin-top: 2px;
}

.close-btn {
	position: absolute;
	right: 5px;
	background: none;
	border: none;
	font-size: 1em;
	color: #a8a4a0;
}

input[type='radio'] {
	-webkit-appearance: none;
	appearance: none;
	margin: 0;
	font: inherit;
	color: currentColor;
	width: 1.15em;
	height: 1.15em;
	border: 0.15em solid currentColor;
	border-radius: 50%;
	transform: translateY(-0.075em);
	display: grid;
	place-content: center;
}

input[type='radio']::before {
	content: '';
	width: 0.65em;
	height: 0.65em;
	border-radius: 50%;
	transform: scale(0);
	transition: 120ms transform ease-in-out;
	box-shadow: inset 1em 1em #9247ae;
	background-color: CanvasText;
}

input[type='radio']:checked::before {
	transform: scale(1);
}
</style>
