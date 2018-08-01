<template>
<el-row type="flex" class="row-bg my4" justify="center">
	<el-col :xs="24" :sm="24" :md="24" :lg="16" :xl="10">
		<div class="grid-content bg-purple">

			<h4 class='subheading-text'>{{ title }}</h4>
			<div class='wrapper-box col-sm-12 '>
				<div class="row">

					<div class="col-sm-6 blck">
						<label class='header-title' for='address'>*Address</label>
						<el-input
							v-validate="{
								required:true, 
								regex: /^([0-9\w\s]+)$/,
								max:255
							}"
							name='address'
							type='text'
							placeholder="Baker Street"
							v-model="address.address"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('address')" class='mb0'>
								{{ errors.first('address') }}
							</p>
						</div>
					</div>

					<div class="col-sm-6 blck">
						<label class='header-title' for='apt'>*Apartment / Suite</label>
						<el-input
							v-validate="{
								required:true, 
								regex: /^([\w\s-]+)$/,
								max:50
							}"
							name='apt'
							type='text'
							placeholder="221B"
							v-model="address.apt"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('apt')" class='mb0'>
								{{ errors.first('apt') }}
							</p>
						</div>
					</div>

					<div class="col-sm-6 blck">
						<label class='header-title' for='city'>*City</label>
						<el-input
							v-validate="'required|alpha|max:255'"
							name='city'
							type='text'
							placeholder="London"
							v-model="address.locality"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('city')" class='mb0'>
								{{ errors.first('city') }}
							</p>
						</div>
					</div>

					<div class="col-sm-6 blck">
						<label class='header-title' for='country'>*Country</label>
						<div>
						    <el-select 
								v-validate="{
									required:true, 
									regex: /^([\w\s-,'\(\)]+)$/,
									max:50
								}"
						    	name='country'
						    	class='full-width-box'
							    v-model="address.country" 
							    filterable 
							    placeholder="Select">
			                    <el-option
			                            v-for="country in countries"
			                            :label="country"
										:key="country"
			                            :value="country">
			                    </el-option>
			    			</el-select>
							<div class="errormsg">
								<p v-show="errors.has('country')" class='mb0'>
									{{ errors.first('country') }}
								</p>
							</div>
						</div>
					</div>

					<div class="col-sm-6 blck" for='region'>
						<label class='header-title'>State / Province</label>
						<el-input
							v-validate="'alpha_spaces'"
							name='region'
							type='text'
							placeholder=""
							v-model="address.region"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('region')" class='mb0'>
								{{ errors.first('region') }}
							</p>
						</div>
					</div>

					<div class="col-sm-6 blck">
						<label class='header-title' for='zip'>*Zip / Postal</label>
						<el-input
							v-validate="{
								required:true, 
								regex: /^([\w-0-9]+)$/,
								min:4,
								max:9
							}"
							name='zip'
							type='text'
							placeholder=""
							v-model="address.zip"></el-input>
						<div class="errormsg">
							<p v-show="errors.has('zip')" class='mb0'>
								{{ errors.first('zip') }}
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
			'address'
		],
		mounted: function (){
			var vm = this;
			console.log('address mounted');
			
			buff.$on('validate-address', async function (last_check_res) {
				let check = await vm.validate(); // boolean
				console.log(check); 
				buff.$emit('validate-finish', check && last_check_res);
			});			
		},
		methods: {
			validate: async function(){
				return await this.$validator.validateAll().then((result) => {
					if (result) {
						console.log("address");
						buff.user.countryName = this.address.country;
						buff.user.locality = this.address.locality;
						buff.user.streetAddress = this.address.address;
						buff.user.extendedAddress = this.address.apt;
						buff.user.region = this.address.region;
						buff.user.postalCode = this.address.zip;

						return true;
					}return false;
				});
			},
		},
		data(){
			return {
				title : "Address",	
				countries: [
					"Afghanistan",
					"Albania",
					"Algeria",
					"Andorra",
					"Angola",
					"Argentina",
					"Armenia",
					"Aruba",
					"Australia",
					"Austria",
					"Azerbaijan",
					"Bahamas",
					"Bahrain",
					"Bangladesh",
					"Barbados",
					"Belarus",
					"Belgium",
					"Belize",
					"Benin",
					"Bhutan",
					"Bolivia",
					"Bosnia and Herzegovina",
					"Botswana",
					"Brazil",
					"Brunei Darussalam",
					"Bulgaria",
					"Burkina Faso",
					"Burundi",
					"Cambodia",
					"Cameroon",
					"Canada",
					"Cabo Verde",
					"Cayman Islands",
					"Central African Republic",
					"Chad",
					"Chile",
					"China",
					"Colombia",
					"Comoros",
					"Congo (Brazzaville)",
					"Congo (Kinshasa)",
					"Cook Islands",
					"Costa Rica",
					"Croatia",
					"Cuba",
					"Curaçao",
					"Cyprus",
					"Czech Republic",
					"Denmark",
					"Djibouti",
					"Dominica",
					"Dominican Republic",
					"Ecuador",
					"Egypt",
					"El Salvador",
					"Equatorial Guinea",
					"Eritrea",
					"Estonia",
					"Ethiopia",
					"Falkland Islands",
					"Faroe Islands",
					"Fiji",
					"Finland",
					"France",
					"French Guiana",
					"French Polynesia",
					"French Southern Lands",
					"Gabon",
					"Gambia",
					"Georgia",
					"Germany",
					"Ghana",
					"Gibraltar",
					"Greece",
					"Greenland",
					"Grenada",
					"Guadeloupe",
					"Guam",
					"Guatemala",
					"Guernsey",
					"Guinea",
					"Guinea-Bissau",
					"Guyana",
					"Haiti",
					"Honduras",
					"Hong Kong",
					"Hungary",
					"Iceland",
					"India",
					"Indonesia",
					"Iran",
					"Iraq",
					"Ireland",
					"Israel",
					"Italy",
					"Jamaica",
					"Japan",
					"Jersey",
					"Jordan",
					"Kazakhstan",
					"Kenya",
					"Kiribati",
					"Korea, North",
					"Korea, South",
					"Kuwait",
					"Kyrgyzstan",
					"Laos",
					"Latvia",
					"Lebanon",
					"Lesotho",
					"Liberia",
					"Libya",
					"Liechtenstein",
					"Lithuania",
					"Luxembourg",
					"Macau",
					"Macedonia",
					"Madagascar",
					"Malawi",
					"Malaysia",
					"Maldives",
					"Mali",
					"Malta",
					"Marshall Islands",
					"Martinique",
					"Mauritania",
					"Mauritius",
					"Mayotte",
					"Mexico",
					"Micronesia",
					"Moldova",
					"Monaco",
					"Mongolia",
					"Montenegro",
					"Montserrat",
					"Morocco",
					"Mozambique",
					"Myanmar",
					"Namibia",
					"Nauru",
					"Nepal",
					"Netherlands",
					"New Zealand",
					"Nicaragua",
					"Niger",
					"Nigeria",
					"Norway",
					"Oman",
					"Pakistan",
					"Palau",
					"Palestine",
					"Panama",
					"Papua New Guinea",
					"Paraguay",
					"Peru",
					"Philippines",
					"Poland",
					"Portugal",
					"Puerto Rico",
					"Qatar",
					"Romania",
					"Russian Federation",
					"Rwanda",
					"Saint Kitts and Nevis",
					"Saint Lucia",
					"Saint Vincent and the Grenadines",
					"Samoa",
					"San Marino",
					"Sao Tome and Principe",
					"Saudi Arabia",
					"Senegal",
					"Serbia",
					"Seychelles",
					"Sierra Leone",
					"Singapore",
					"Sint Maarten (Dutch part)",
					"Slovakia",
					"Slovenia",
					"Solomon Islands",
					"Somalia",
					"South Africa",
					"South Sudan",
					"Spain",
					"Sri Lanka",
					"Sudan",
					"Suriname",
					"Swaziland",
					"Sweden",
					"Switzerland",
					"Syria",
					"Taiwan",
					"Tajikistan",
					"Tanzania",
					"Thailand",
					"Timor-Leste",
					"Togo",
					"Tonga",
					"Trinidad and Tobago",
					"Tunisia",
					"Turkey",
					"Turkmenistan",
					"Tuvalu",
					"Uganda",
					"Ukraine",
					"United Arab Emirates",
					"United Kingdom",
					"United States of America",
					"Uruguay",
					"Uzbekistan",
					"Vanuatu",
					"Venezuela",
					"Vietnam",
					"Yemen",
					"Zambia",
					"Zimbabwe",
				],
			
			}
		}
	}
</script>


<style lang='scss' >

</style>
