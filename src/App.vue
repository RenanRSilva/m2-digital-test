/* eslint-disable */
<template>
	<div class="container">
		<h1>Internet</h1>
		<p v-if="isLoading">Carregando...</p>
		<p class="error" v-if="requestError">
			{{ requestError }}
		</p>
		<p>Selecione um plano de internet para continuar</p>
		<internet-card-row
			@update-internet-selected="setInternet"
			:internetProducts="this.products.internet"
		>
		</internet-card-row>
		<h1>TV</h1>
		<p>Selecione um plano de TV</p>
		<tv-card-row
			@update-selected-tv="setTv"
			:internetIsSelected="internetIsSelected"
			:tvProducts="this.products.tv"
		>
		</tv-card-row>
		<h1>Fixo</h1>
		<p>Agora escolha seu pacote de telefone fixo</p>
		<landline-card-row
			@update-selected-landline="setLandline"
			:internetIsSelected="internetIsSelected"
			:landlineProducts="this.products.fixo"
		>
		</landline-card-row>
		<base-footer :total-price="totalPrice"></base-footer>
	</div>
</template>

<script>
import axios from 'axios'
import BaseFooter from './components/BaseFooter.vue'
import InternetCardRow from './components/InternetCardRow.vue'
import TvCardRow from './components/TvCardRow.vue'
import LandlineCardRow from './components/LandlineCardRow.vue'

export default {
	name: 'App',
	components: {
		BaseFooter,
		InternetCardRow,
		TvCardRow,
		LandlineCardRow
	},
	data() {
		return {
			products: {},
			tvPrice: 0,
			internetPrice: 0,
			landlinePrice: 0,
			requestError: '',
			isLoading: false
		}
	},
	methods: {
		setInternet(internetPrice) {
			this.internetPrice = internetPrice
			this.internetIsSelected = true
		},
		setTv(tvPrice) {
			this.tvPrice = tvPrice
		},
		setLandline(landlinePrice) {
			this.landlinePrice = landlinePrice
		}
	},
	computed: {
		totalPrice() {
			return this.internetPrice + this.tvPrice + this.landlinePrice
		},
		internetIsSelected() {
			return this.internetPrice
		}
	},
	mounted() {
		this.isLoading = true
		axios
			.get(
				'https://my-json-server.typicode.com/RenanRSilva/m2-digital-test/db'
			)
			.then((data) => {
				this.isLoading = false
				this.products = data.data
			})
			.catch((err) => {
				this.isLoading = false
				this.requestError = `Algo deu errado, tente novamente mais tarde! ${err}`
			})
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: start;
	margin-top: 30px;
}
*,
*:before,
*:after {
	box-sizing: border-box;
}

body {
	margin: 0;
}

h1 {
	margin-bottom: 5px;
	color: #9247ae;
}

p {
	color: #a8a4a0;
	margin-top: 5px;
}

ul {
	padding-left: 0;
}
.container {
	width: 1140px;
	margin: 0 auto;
}

.error {
	color: rgb(184, 13, 13);
}
.card {
	display: flex;
	flex-direction: column;
	list-style: none;
	padding: 15px 10px 10px 10px;
	border: 1px solid #a8a4a0;
	border-radius: 7px;
	width: 200px;
	margin-right: 10px;
	position: relative;
}

@media (min-width: 1500px) {
	.container {
		min-width: 1400px;
	}
}

@media (min-width: 1200px) {
	.container {
		min-width: 900px;
	}
}
@media (min-width: 992px) {
	.container {
		width: 960px;
	}
}
@media (min-width: 768px) {
	.container {
		width: 720px;
	}
}

@media (max-width: 576px) {
	.container {
		width: 100%;
	}
	.card {
		margin-top: 15px;
		width: 250px;
		margin-right: 0px;
	}
	p,
	h1,
	h2,
	h3,
	h4 {
		padding-left: 20px;
		padding-right: 10px;
	}
}
</style>
