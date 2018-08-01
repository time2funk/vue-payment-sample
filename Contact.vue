<template>
<el-row type="flex" class="row-bg my4" justify="center">
	<el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="10">
		<div class="grid-content bg-purple">
		
			<h4 class='subheading-text'>{{ title }}</h4>

			<div class='wrapper-box col-sm-12 '>
				<div class="row">
					<div class="col-sm-6 blck ">
						<label class='header-title' for='firstName'>*First Name</label>
						<el-input
							v-validate="'required|alpha|min:3|max:50'"
							data-vv-as='first name'
							name='firstName'
							type='text'
							placeholder="First Name"
							v-model="contact.firstName"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('firstName')" class='mb0'>
								{{ errors.first('firstName') }}
							</p>
						</div>
					</div>
					<div class="col-sm-6 blck">
						<label class='header-title' for='lastName'>*Last Name</label>
						<el-input
							v-validate="'required|alpha|min:3|max:50'"
							data-vv-as='last name'
							name='lastName'
							type='text'
							placeholder="Last Name"
							v-model="contact.lastName"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('lastName')" class='mb0'>
								{{ errors.first('lastName') }}
							</p>
						</div>
					</div>
					<div class="col-sm-12 blck">
						<label class='header-title' for='company'>*Company Name</label>
						<el-input
							v-validate="'required|alpha_spaces|min:2'"
							name='company'
							type='text'
							placeholder="Company Name"
							v-model="contact.company"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('company')" class='mb0'>
								{{ errors.first('company') }}
							</p>
						</div>
					</div>
					<div class="col-sm-12 blck">
						<label class='header-title' for='email'>*E-Mail Address</label>
						<el-input
							v-validate="'required|email|max:255'"
							name='email'
							type='email'
							placeholder="E-Mail Address"
							v-model="contact.email"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('email')" class='mb0'>
								{{ errors.first('email') }}
							</p>
						</div>
					</div>
				</div>
			</div>

		</div>
	</el-col>
</el-row>
</template>


<script>
	import { buff } from '@/views/components/plans/buff.js'

	export default {
		props:[
			'contact'
		],
		data(){
			return {
				title : "Contact",
			}
		},
		methods: {
			validate: async function(){
				return await this.$validator.validateAll().then((result) => {
					if (result) {
						console.log("contact");
						buff.user.firstName = this.contact.firstName;
						buff.user.lastName = this.contact.lastName;
						buff.user.company = this.contact.company;
						buff.user.email = this.contact.email;
						
						return true;
					}return false;
				});
			},
		},
		mounted: function (){
			var vm = this;
			console.log('contact mounted');
			
			buff.$on('validate-contact', async function (last_check_res) {
				let check = await vm.validate(); // boolean
				console.log(check); 
				buff.$emit('validate-address', check && last_check_res);
			});
		},
	}
</script>


<style lang='scss' >

</style>
