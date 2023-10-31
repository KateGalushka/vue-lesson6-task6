<template>
	<div class="container">
		<div class="card-number">
			<label>
				<p>CARD NUMBER</p>
				<input
				 class="card-number__input"
				 type="text"
				 pattern="[0-9\s]{13,19}"
				 maxlength="19"
				 placeholder="1234 1234 1234 1234"
				 ref="cardValue"
				 :key="updateKey1"
				 v-model="numberValue"
				 >
			</label>
		</div>
		<div class="container__bottom">
			<label>
				<p>EXPIRY DATE</p>
				<input
				 class="card-date__input"
				 type="text"
				 pattern="\d{4}"
				 maxlength="4"
				 placeholder="MM / YY"
				 ref="dateValue"
				 :key="updateKey2"
				 v-model="dateValue"
				 > 
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
				updateKey2: 0,

			}
		},
		computed: {
			dateValue: {
				get(){
					return this.expiryDate
				},
				set(val){
					val= val.replace(/\D/g, '');
					this.$nextTick(() => {
						this.updateKey2++;
						this.$nextTick(() => {
						this.$refs.dateValue.focus()
						})
					});
					val = val.replace(/(\d{2})(\d{2})/, "$1/$2");
					
					this.$emit('update:expiryDate', val)
				}
			},
			numberValue:{
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
}
.card-code__input{
	background: url('.././assets/question.jpg') right 8px center no-repeat, #fff;
	background-size: contain;
	max-width: 120px;
}

</style>