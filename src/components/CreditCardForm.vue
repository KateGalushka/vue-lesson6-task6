<template>
	<div class="container">
		<div class="card-number">
			<label>
				<p>CARD NUMBER</p>
				<input
				 v-model="cardNumberValue"
				 class="card-number__input"
				 type="text"
				 pattern="[0-9\s]{13,19}"
				 maxlength="19"
				 placeholder="1234 1234 1234 1234"
				 ref="cardValue"
				 :key="updateKey1"
				 @keydown="onKeyDown"
				 >
			</label>
		</div>
		<div class="container__bottom">
			<label>
				<p>EXPIRY DATE</p>
				<input
				 v-model="expDateValue"
				 class="card-date__input"
				 type="text"
				 pattern="\d{4}"
				 maxlength="5"
				 placeholder="MM / YY"
				 @keydown="onKeyDown"
				 > 
				 <div v-if="showMessage">Введіть коректне значення місяця (від 01 до 12)</div>
			</label>	
			
			<label>
				<p>SECURE CODE</p>
				<input class="card-code__input" type="text" maxlength="3" v-model="codeValue">
			</label>
		</div>

	</div>
</template>

<script>
	export default {
		name: 'CreditCardForm',
		props: {
			cardNumber: {
				type: String,
				default: null
			},
			expiryDate: {
				type: String,
				default: null
			},
			securityCode: {
				type: String,
				default: null
			}
		},
		data() {
			return {
				updateKey1: 0,
				showMessage: false
			}
		},
		computed: {
			expDateValue: {
				get(){
					return this.expiryDate
				},
				set(val){
					this.$emit('update:expiryDate', val)
				}
			},
			cardNumberValue:{
				get(){
					return this.cardNumber
				},
				set(newVal){
					newVal = newVal.replace(/\D/g, '');
					this.$nextTick(() => {
						this.updateKey1++;
						this.$nextTick(() => {
						this.$refs.cardValue.focus()
						})
					});
					newVal = newVal.replace(/(\d{4}(?=.+))/g, '$1 ');
					this.$emit('update:cardNumber', newVal)
				}
			},
			codeValue: {
				get(){
					return this.securityCode
				},
				set(val){
					this.$emit('update:securityCode', val)
				}
			}
		},

		watch: {
			expDateValue(newValue, oldValue) {
				if (newValue.length === 2 && oldValue.length == 1) {
					this.expDateValue = this.expDateValue + '/';
				} else if (newValue.length === 2 && oldValue.length === 3) {
					this.expDateValue = newValue[0];
				};
				(newValue[0]+newValue[1] > 12)? this.showMessage = true: this.showMessage = false
			},
		},
		methods: {
			onKeyDown(event) {
				console.log(event.key)
				const key = event.key;
				const isDigit = key >= '0' && key <= '9';
				if (!isDigit && key !== 'Backspace') {
					event.preventDefault();
				}
			}
		},
	}
</script>

<style lang="scss" scoped>
*{
	box-sizing: border-box;
}
.container{
	width: 350px;
	margin: 50px auto;
	padding: 10px;
	border: 1px solid grey;
	border-radius: 5px;
	font-size: 16px;
	background-color: azure;

}
label{
	input{
		border: 0.5px solid grey;
		border-radius: 8px;
		outline: none;
		padding: 10px 15px;
		display: inline-block;
	}
	p{
		margin: 0 0 10px;
	}
}
.card-number{
	margin-bottom: 20px;
}
.card-number__input{
	background: url('.././assets/visa.svg') right 8px center no-repeat, #fff;
	background-size: contain;
	width: 100%;
}
.container__bottom{
	display: flex;
	gap: 10px;
	margin-bottom: 10px;
	div{
		color: red;
	}
}
.card-code__input{
	background: url('.././assets/question.jpg') right 8px center no-repeat, #fff;
	background-size: contain;
	max-width: 120px;
}

</style>