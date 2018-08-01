<template>
<el-row type="flex" class="row-bg my4" justify="center">
	<el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="10">
		<div class="grid-content bg-purple">

			<h4 class='subheading-text'>{{ title }}</h4>

			<el-row type="flex" class="row-bg" justify="center">
				<el-col :xs="24" :sm="12"
					v-for='frequency in transform(frequencies)'>
					<div class="grid-content bg-purple mx">
						<div class='wrapper-box col-sm-12 check-block '
		      				v-on:click='changeFrequency( frequency.id, frequency.disabled )'
		      				v-bind:class='{ 
		      					active: frequency.id == curFreq,
		      					disabled: frequency.disabled ? true : false
		      				}'>
							<div class="row pl4 relpos">
								<div class="check-box">
									<div class="circle">
										<div class="point"></div>
									</div>
								</div>
								<div class="col-sm-12">
									<h4>{{frequency.type}}</h4>
									<p class='gray-text mb0'>
										{{euro(frequency.price)}} per seat per month
									</p>
								</div>
								<div class='tab-box'
									v-if='frequency.save'>
									<div class='angl'>
										<div class='top-angl'></div>
										<div class='bot-angl'></div>
									</div>
									<div class='tab-box-body'>
										save
										<br>
										<span>{{euro(frequency.save)}}</span>
									</div>

								</div>
							</div>
						</div>
					</div>					
				</el-col>
			</el-row>

		</div>
	</el-col>
</el-row>
</template>


<script>
	export default {
		props: [
			"frequencies",
			"curFreq",
		],
		data(){
			return {
				title : "choose your biling frequency",
			}
		},
		methods: {
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
			transform(frequencies){
				let max_val = 0;

				frequencies.forEach((frequency,i)=>{
					if(frequency.price > max_val)
						max_val = frequency.price;
				});
				frequencies.forEach((frequency,i)=>{
					if(frequency.price < max_val)
						frequencies[i].save = Math.round( (max_val - frequency.price) * 12 )
				});
				return frequencies;
			},
			equal(e1,e2){
				return e1 == e2
			},
			euro(value){
				return '€'+value+''
			},
			changeFrequency(id, disabled){
				// this.bill_flag = id;
				// this.curFreq = id;
				if(disabled) return;
				this.$emit('chosenFreq', id);
				console.log(id);
			}
		}
	}
</script>


<style lang='scss'>
	.tab-box{
		position: absolute;
		top: -20px;
		right: -15px;
		color: white;
		font-weight: bold;
		letter-spacing: 1px;
		box-shadow: 0px 1px 1px -2px black, 0px 3px 2px -3px #000;
		border-top-right-radius: 3px;
		border-bottom-right-radius: 3px;
		.angl{
			float: left;
			.top-angl{
				width: 0;
				height: 0;
				border-top: 25px solid #68B8AE;
				border-left: 10px solid transparent;
			}
			.bot-angl{
				width: 0;
				height: 0;
				border-bottom: 25px solid #68B8AE;
				border-left: 10px solid transparent;
			}
		}
		.tab-box-body{
			line-height: 1.5;
			text-transform: uppercase;
			float: right;
			background: #68B8AE;
			height: 50px;
			padding: 3px 20px;
			border-top-right-radius: 2px;
			border-bottom-right-radius: 2px;
			span {
			    font-size: 18px;
			    font-weight: initial;
			}
		}
	}
	.wrapper-box{
		&.check-block{    
			border: 1px solid white;
			position: relative;
			cursor: pointer;

			&.active{
				border: 1px solid #90ca90;
				top: -2px;
				box-shadow: 0px 4px 6px -3px #959595;
				// transition: 0.5s;
				.check-box{

					.circle {

						.point{
							background: #90ca90;
						}
					}
				}
			}
			.check-box {
				position: absolute;
			    left: 10px;
			    height: 100%;
			    width: 21px;
			    font-size: 25px;
			    top: calc(50% - 14px);
			    display: block;
			    color: #90ca90;
			    line-height: 1;

			    .circle {
				    height: 21px;
				    width: 21px;
				    border-radius: 50%;
				    padding: 4px;
				    border: 2px solid #90ca90;

				    .point{
				    	background: transparent;
				    	border-radius: 50%;
						width: 100%;
						height: 100%;
				    }
				}
			}
		}
	}
</style>
