<template>
<el-row type="flex" class="row-bg my4" justify="center">
	<el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="10">
		<div class="grid-content bg-purple">

			<h4 class='subheading-text'>
				{{ title }}
			</h4>

			<div class='wrapper-box col-sm-12 '>
				<div class="row ">
					<div class="col-sm-8">
						<h4 class='subheading my'>
							1 seat
						</h4>
						<p class='body-1 my grey-text'>
							{{
								freq
								? "Paid Plan Yearly"
								: "Paid Plan Monthly"
							}}
						</p>
					</div>
					<div class="col-sm-4 right-text">
						<h4 class='title my'>
							{{ 
								freq 
								? euro(seat*12)
								: euro(seat)
							}}
						</h4>
						<p class='body-1 my grey-text'>
							{{
								freq 
								? "Every Year"
								: "Every Month"
							}}
						</p>
					</div>
				</div>

				<hr class="dot">

				<div class="row ">
					<div class="col-sm-8">
						<h4 class='subheading my'>
							VAT
						</h4>
					</div>
					<div class="col-sm-4 right-text">
						<h4 class='title my'>
							{{euro(vat)}}
						</h4>
					</div>
				</div>

				<hr class="dot">

				<div class="row mt">
					<div class='wrapper-box col-sm-12 o-border o-back'>
						<div class="row">
							<div class="col-sm-8">
								<h4 class=''>
									Order Total
								</h4>
								<p class='grey-text mb0'>
									{{
										freq 
										? "Paid Plan for 12 seats with yearly payment" 
										: "Paid Plan for 1 seat with monthly payment"
									}}
								</p>
							</div>	
							<div class="col-sm-4 right-text">
								<h3 class='lh2 mb0'>
									{{ euro( math() ) }}
								</h3>
							</div>		
						</div>
					</div>
				</div>
			</div>

		</div>
	</el-col>
</el-row>
</template>


<script>
	export default {
		props: [
			'freq',
			'vat',
			'seat',
		],
		data(){
			return {
				title : "order details"
			}
		},
		methods: {
			euro(value){
				return '€'+value+''
			},
			math: function(){
				if( this.freq )
					return ((this.vat + this.seat)*12).toFixed(2)
				else
					return ((this.vat + this.seat)).toFixed(2)
			},
		},
	}
</script>


<style lang='scss' >
</style>
