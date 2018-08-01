<template>
	<el-row type="flex" class="row-bg my4" justify="center">
		<el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="10">
			<div class="grid-content bg-purple payment-section">

				<h4 class='subheading-text'>{{ title }}</h4>
				<b-card no-body>
					<b-tabs>
						<b-tab title="<div class='pay-title'><i class='fa fa-credit-card fa-2x'></i><br/>Credit card</div>" class="pay-tab">
					    <b-card border-variant="light"
					            header="
									<div class='pay-title'>Accepted credit card</div><br/>
									<i class='fa fa-cc-mastercard fa-4x'></i>
									&emsp;
									<i class='fa fa-cc-visa fa-4x'></i>
									<hr/>
									We do not accept POs, checks, or invoices to be paid at a later date. We will email you a receipt each time you are charged.
								"
					            class="text-center"
											no-body>

									<form action="/" id="aid-payment-form" method="post">
									<div class="row">
										<div class="col-sm-6 blck">
											<label class='header-title' for='cardFirstName'>*First Name</label>
											<el-input
												name='cardFirstName'
												type='text'
												placeholder="First Name"
												v-validate="'required|alpha|min:3'"
												data-vv-as='First Name'
												v-model="nameFirst"></el-input>
											<div class="errormsg">
												<p v-show="errors.has('cardFirstName')" class='mb0'>
													{{ errors.first('cardFirstName') }}
												</p>
											</div>
										</div>
										<div class="col-sm-6 blck">
											<label class='header-title' for='cardLastName'>*Last Name</label>
											<el-input
												name='cardLastName'
												type='text'
												placeholder="Last Name"
												v-validate="'required|alpha|min:3'"
												data-vv-as='First Name'
												v-model="nameLast"></el-input>
											<div class="errormsg">
												<p v-show="errors.has('cardLastName')" class='mb0'>
													{{ errors.first('cardLastName') }}
												</p>
											</div>
										</div>

										<!-- hostedfields -->
										<div class="col-sm-12 blck">
											<label class='header-title'  for="card-number">*Credit card number</label>
											<div id="card-number" class="hosted-field"></div>
										</div>

										<!-- hostedfields -->
										<div class="col-sm-6 blck">
											<label class='header-title' for='cvv'>*CVV</label>
											<div id="cvv" class="hosted-field"></div>
										</div>

										<!-- hostedfields -->
										<div class="col-sm-6 blck">
												<label class='header-title' for="expiration-date">*Expiration Date</label>
												<div id="expiration-date" class="hosted-field"></div>
										</div>
									</div>
									<br/>
									<b-button 
										id='aid-payment-form-submit' 
										disabled 
										class="w-100" 
										type="submit" 
										variant="success">
										{{ 
											braintree 
											? "Update Billing Details" 
											: "Save Billing Details" 
										}}
									</b-button>
									</form>
					    </b-card>
						</b-tab>
						<b-tab title="<div class='pay-title'><i class='fa fa-paypal fa-2x'></i><br/>Paypal</div>" class="pay-tab">
					    <b-card border-variant="light"
					            header="
												<div class='pay-title'>Paypal</div>
												<hr/>
												We do not accept POs, checks, or invoices to be paid at a later date. We will email you a receipt each time you are charged.
											"
					            class="text-center"
											no-body>
					      <p class="card-text">** Paypal Form Here **</p>
					    </b-card>
						</b-tab>
					</b-tabs>
				</b-card>

			</div>
		</el-col>
	</el-row>
</template>


<script>
	import { buff } from '@/views/components/plans/buff.js'

	export default {
		props: [
			'braintree'
		],
		data(){
			return {
				title : "Payment",
				nameFirst: null,
				nameLast: null,
			}
		},
		methods: {
			validate: async function(){
				return await this.$validator.validateAll().then((result) => {
					if (result) {
						console.log("payment");
						buff.user.cardNameFirst = this.nameFirst;
						buff.user.cardNameLast = this.nameLast;

						return true;
					}return false
				});
			},
		},
		mounted: function (){
			var vm = this;
			buff.$on('validate', async function (last_check_res) {
				let check = await vm.validate(); // boolean
				console.log(check); 
				buff.$emit('validate-contact', check && last_check_res);
			});

			console.log('payment mounted');
		},
	}
</script>


<style lang='scss'>
.payment-section{
	input{
		color: #141414;
	}
	.pay-tab {
		padding: 0 !important;
	}
	li.nav-item {
		width: 50%;
	}
	.hosted-field{
		height: 40px;
		border: 1px solid #dcdfe6;
		border-radius: 5px;
		padding: 9px 15px;
	}
	.pay-title {
		text-align: center;
		font-size: 18px;
		text-transform: uppercase;

		i {
			margin: 15px auto;
		}
	}
	.blck {
		text-align: left;
		margin: 10px 0;

		.header-title {
			color: #808080;
		}
	}
}
</style>
