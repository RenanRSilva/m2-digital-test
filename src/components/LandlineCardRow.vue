<template>
	<card-container>
		<li
			:class="inputDisabled && 'disabled-card'"
			v-for="product in landlineProducts"
			v-bind:key="product.id"
			class="card"
		>
			<input
				:id="product.name"
				:value="product.id"
				type="radio"
				:disabled="inputDisabled"
				v-model="selectedLandlineId"
			/>
			<p class="service-title">
				{{ product.name.toUpperCase() }}
			</p>
			<p class="service-price">
				{{
					product.price.toLocaleString('pt-br', {
						style: 'currency',
						currency: 'BRL'
					})
				}}
			</p>
			<button
				class="close-btn"
				v-if="product.id === selectedLandlineId"
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
	name: 'landline-card-row',
	emit: ['update-selected-landline'],
	components: {
		cardContainer
	},
	props: {
		internetIsSelected: {
			type: Number,
			required: true
		},
		landlineProducts: {}
	},
	data() {
		return {
			selectedLandlineId: 0,
			inputDisabled: true,
			landlinePrice: 0
		}
	},
	methods: {
		removeFromCart: function () {
			this.selectedLandlineId = 0
			this.landlinePrice = 0
			this.reloadInput = !this.reloadInput
			this.reloadInput = !this.reloadInput
			this.$emit('update-selected-landline', this.landlinePrice)
		}
	},
	watch: {
		inputDisabled() {
			if (this.inputDisabled) {
				this.landlinePrice = 0
				this.selectedLandlineId = 0
				this.$emit('update-selected-landline', this.landlinePrice)
			}
		},
		internetIsSelected() {
			this.inputDisabled = !this.internetIsSelected
		},
		selectedLandlineId() {
			if (this.selectedLandlineId) {
				const selectedLandline = this.landlineProducts.filter(
					(product) => this.selectedLandlineId === product.id
				)
				const landlineObj = JSON.parse(JSON.stringify(selectedLandline))[0]
				this.landlinePrice = landlineObj.price
				this.$emit('update-selected-landline', this.landlinePrice)
			}
		}
	}
}
</script>
<style scoped>
p strong {
	color: black;
}

.service-title {
	font-size: 1.4em;
	color: black;
	margin-top: 5px;
}

.service-price {
	color: black;
	margin-bottom: 5px;
}
.card-container {
	display: flex;
	flex-direction: row;
	justify-content: start;
	align-items: start;
	margin-top: 25px;
	margin-bottom: 50px;
}

.details {
	text-align: right;
}

.close-btn {
	position: absolute;
	right: 5px;
	background: none;
	border: none;
	font-size: 1em;
	color: #a8a4a0;
}

.disabled-card {
	background-color: rgba(231, 231, 231, 0.514);
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
@media (max-width: 576px) {
	.card-container {
		align-items: center;
		flex-direction: column;
		margin-top: 5px;
	}
}
</style>
