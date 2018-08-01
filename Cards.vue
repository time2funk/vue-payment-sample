<template>
<div class="p-section">
	<div class='container animated fadeIn my2'>
		<div class="row">
			<div class="col-sm-12">	
				<h4 class='bold-text center-text'>{{ upperCase(title) }}</h4>
			</div>

      		<div v-for='plan in counterbalance(plans)'
      			v-show='showCards || plan.id'
      			v-bind:class="{
      				'centered': !showCards,
      				'col-sm-10': !showCards && cur_plan == plan.id,
      				'col-sm-6': showCards
				}"> <!-- ! -->
      			<div class="pointer-card my"
      				v-on:click='changePlan( plan.id )'
      				v-bind:class='{ active: cur_plan == plan.id }'>
      				
					<div class="p-header center-text">
						<h3 v-if='plan.id' 
							class='black-text'>
							{{upperCase(plan.name)}}
						</h3>
						<h3 v-else
							class='grey-text'>
							{{upperCase(plan.name)}}
						</h3>

						<label class="green-text">
							{{upperCase(plan.sub_title)}}
						</label>
					</div>
					<div class="p-body">
						<ul class='p-list center-text'>
							<li class='p-list-item'
								v-for='feature in plan.features'>
								<p class='body-1-text'>
									{{upperCase(feature)}}
								</p>
							</li>
						</ul>
					</div>
					<div class="p-footer"
						v-show='showCards'>
						<div class="text-center" >
							<p class='money grey-text'>
								{{ checkMoneyValue(plan.price) }}
							</p>
							<p class='body-2'>seat / month</p>
							<p class='caption'>( plus applicable tax )</p>
						</div>
					</div>
      			
      			</div>
      		</div>



		</div>		
	</div>
</div>
</template>


<script>
	export default {
		props:[
			'plans',
			'cur_plan',
			'showCards',
		],
		data(){
			return {
				title: this.showCards ? "update your billing details" : "your billing details",
				plan_flag: null,
				// classObject : 
			}
		},
		methods: {
			counterbalance(plans){
				let max_featur_length = 0;

				plans.forEach((plan, i)=>{
					if( plan.features.length >  max_featur_length)
						max_featur_length = plan.features.length;
				});
				plans.forEach((plan, i)=>{
					if( plan.features.length < max_featur_length){
						let diff = max_featur_length - plan.features.length;
						for(let j=0; j<diff; j++){
							plans[i].features.push('');
						}
					}
				});
				return plans;
			},
			upperCase(text){
				if(text == '' || text == null) return ''
				else{
					let arr = text.split(/\s/);
					let res = '';
					for(let i=0;i<arr.length; i++){
						res += arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
						if(i<arr.length-1) res += ' ';
					}
					return res;
				}
			},
			upperCaseFirst(text){
				if(text == '' || text == null) return ''
				else{
					text = text.toString();
					let arr = text.split(/\s/);
					let res = '';
					for(let i=0;i<arr.length; i++){
						res += arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
						if(i<arr.length-1) res += ' ';
					}
					return res;
				}
			},
			append(text, text2){
				return '' + text + text2
			},
			euro(value){
				return '€'+value+''
			},
			equal(e1,e2){
				return e1 == e2
			},
			changePlan(id){
				// if(this.plan_flag == null)
					if(this.cur_plan == id) return
					else {
						if(id)
							this.$confirm('Subscribe to the Paid Plan?', '', {
								confirmButtonText: 'Yes',
								cancelButtonText: 'Cancel',
							}).then((action) => {
								if(action == 'confirm'){
									this.plan_flag = id;
									this.$emit('chosenPlan', this.plan_flag);
								}
							}).catch(() => {
								// do nothing
								// this.$message({
								// type: 'info',
								// message: 'Delete canceled'
							}); 
						else{
							this.plan_flag = id;
							this.$emit('chosenPlan', this.plan_flag);
						}
					}


			},
			checkMoneyValue: function(val){
				if( val && val != '0')
					return this.euro(val)
				else
					return "free"
			},
		},
		mounted: function(){
			console.log('cards mounted');
		},
		created: ()=>{
			console.log('cards created');
		},
	}
</script>

<style lang='scss'>
    
	.p-header{
	    padding-top: 10px;

		h3 {
		    // display: flex;
		    // flex-wrap: wrap;
		    padding: 16px;
		    margin: 0;
		    line-height: 1;
		    letter-spacing: .02em;
			font-size: 20px;
			font-weight: 500;
		}
		label {
			// display: flex;
		    padding: 16px;
		    font-size: 14px;
		    font-weight: 600;
		}
	} 

	.p-footer {
	  box-shadow: inset 0 0 8px 0px #b5b5b5;
	}

	ul.p-list{
		list-style: none;
		margin: 0;
		padding: 0;

		li.p-list-item {
		    border: 1px solid #e1e1e1;
		    margin-bottom: -1px;
		    padding: 16px;

			p {
			    margin: 0;
				height: 21px;
			}
		}
	}

	.pointer-card{
		position: relative;
		cursor: pointer;
		border: 2px solid white;
    	border-radius: 4px;
	    background-color: #fff;
	    color: rgba(0,0,0,.87);
    	box-shadow: 0 2px 1px -1px rgba(0,0,0,.2), 0 1px 1px 0 rgba(0,0,0,.14), 0 1px 3px 0 rgba(0,0,0,.12);

    	p{
    		margin: 0;
    	}
		.p-footer {
		    // margin: auto -2px;
		    // margin-bottom:  -2px;
		    border: 2px solid white;
			background: #f5f5f5;
		    margin: 0 -2px;
		    margin-bottom: -1px;
			margin-top: 1px;
		    padding: 16px 0;
		    border: 2px solid white;
		    border-radius: 2px;
		}
		&:hover{
			border: 2px solid #e5f0e6;
	  		transition: 0.3s;
			.p-footer {
				border: 2px solid #e5f0e6;
			}
		}
		&.active{
			top:-2px;
			border: 2px solid #8fc88f;
		    transition: 0.3s;
		    box-shadow: 0px 2px 10px 0px rgba(0, 0, 0, 0.34901960784313724);

	    	.p-footer {
			    border: 2px solid #8fc88f;
			}
		}
	}
</style>
