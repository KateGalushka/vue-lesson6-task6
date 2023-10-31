<template>
	<credit-card-form
	 v-model:cardNumber="cardNumberValue"
	 v-model:expiryDate="cardDateValue"
	 v-model:securityCode="codeValue" />
	<div> Card number: {{ cardNumberValue }}</div>
	<div> Expiry date: {{ cardDateValue }}</div>
	<div> Security code: {{ codeValue }}</div>
</template>

<script>
import CreditCardForm from './components/CreditCardForm.vue';

export default {
	name: "App",
	components: {
		CreditCardForm
	},
	data() {
		return {
			cardNumberValue: null,
			cardDateValue: null,
			codeValue: null
		}
	}
}
</script>

<style lang="scss"></style>
