<template>
	<v-container class="mt-10">
		<v-row>
			<v-col class="hidden-xs-only" sm="2" md="3"></v-col>
			<v-col>
				<!-- Main form -->
				<v-form v-model="valid">
					<v-card loutlined>
						<!-- TITLE -->
						<v-card-title class="mb-5">
							<h4 class="text-h6 mx-auto">
							Form
							</h4>
						</v-card-title>
						<div class="input-container">
							<!-- First name input -->
							<v-text-field
								v-model="values.firstName"
								:rules="stringRules"
								label="First Name*"
								placeholder="Max"
								outlined
								required
							>
							</v-text-field>
							<!-- Last name input -->
							<v-text-field
								v-model="values.lastName"
								:rules="stringRules"
								label="Last Name*"
								placeholder="Mustermann"
								outlined
								required
							>
							</v-text-field>
							<!-- Birthday -->
							<v-menu
								ref="menu"
								v-model="menu"
								:close-on-content-click="false"
								transition="scale-transition"
								offset-y
								min-width="auto"
							>
								<template v-slot:activator="{ on, attrs }">
									<v-text-field
										v-model="date"
										label="Date of Birth*"
										:rules="stringRules"
										append-icon="mdi-calendar"
										readonly
										outlined
										v-bind="attrs"
										v-on="on"
									>
									</v-text-field>
								</template>
								<v-date-picker
									v-model="date"
									:active-picker.sync="activePicker"
									:max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
									min="1950-01-01"
									@change="save"
								>
								</v-date-picker>
							</v-menu>
							<!-- Select primary email -->
							<v-select
								v-model="selectedPrimaryEmail"
								:items="primaryEmail"
								label="Primary Email*"
								outlined
							>
							</v-select>
							<!-- Business email -->
							<v-text-field
								v-model="values.email[0].value"
								:rules="businessEmailRules"
								label="Business E-mail*"
								placeholder="example@email.com"
								outlined
								required
							>
							</v-text-field>
							<!-- Private email -->
							<v-text-field
								v-model="values.email[1].value"
								:rules="privateEmailRules"
								label="Private E-mail"								
								placeholder="example@email.com"
								outlined
							>
							</v-text-field>
							<!-- Street input -->
							<v-text-field
								v-model="values.street"
								:rules="stringRules"
								label="Street*"
								placeholder="Second"
								outlined
								required
							>
							</v-text-field>
							<!-- Zip code -->
							<v-text-field
								v-model.number="values.zip"
								:rules="zipRules"
								label="Zip code*"
								placeholder="12345"
								outlined
								required
							>
							</v-text-field>
							<!-- City input -->
							<v-text-field
								v-model="values.city"
								:rules="stringRules"
								label="City*"
								placeholder="Berlin"
								outlined
								required
							>
							</v-text-field>
						</div>
						<div class="bottom">
							<p>* = required</p>
							<v-btn
								:disabled="!valid"
								elevation="2"
								class="mb-5 ml-5"
								color="primary"
								@click="printValues"
							>
								Print
							</v-btn>
						</div>
					</v-card>
				</v-form>
			</v-col>
			<v-col class="hidden-xs-only" sm="2" md="3"></v-col>
		</v-row>
	</v-container>
</template>

<script>
export default {
	name: "form",
	data() {
		return {
			activePicker: null,
			date: null,
			menu: false,
			valid: false,
			values: {
				firstName: '',
				lastName: '',
				birthdate: '',
				email: [
					{
						type: 'business',
						value: '',
						primary: true,
					},
					{
						type: 'private',
						value: '',
						primary: false,
					},
				],
				street: '',
				zip: '',
				city: '',
			},
			primaryEmail: ['Business', 'Private'],
			selectedPrimaryEmail: 'Business',
			businessEmailRules: [
				v => !!v || 'This field is required!',
				v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
			],
			privateEmailRules: [
				v => /.+@.+\..+/.test(v) || !v || 'E-mail must be valid',
			],
			zipRules: [
				v => !!v || 'This field is required!',
				v => !isNaN(v) || 'Zip code is invalid!'
			],
			stringRules: [
				v => !!v || 'This field is required!',
			],
		};
	},
	watch: {
		// Whenever selectedPrimaryEmail changes, will change values.emails[x].primary
		selectedPrimaryEmail: function(value) {
			if (value === 'Private') {
				this.values.email[0].primary = false;
				this.values.email[1].primary = true;
			} else {
				this.values.email[0].primary = true;
				this.values.email[1].primary = false;
			}
		},
		// watches birthday picker
		menu (val) {
			val && setTimeout(() => (this.activePicker = 'YEAR'))
		},
	},
	methods: {
		// Prints input values in the console!
		printValues() {
			console.log(JSON.stringify(this.values));
		},
		// saves and sends the date to parent element
		save (date) {
			this.$refs.menu.save(date);
			this.values.birthdate = date;
		},
	},
}
</script>

<style>
.input-container {
	padding: 0 20px;
}

.bottom {
	display: flex;
	justify-content: space-between;
    align-items: center;
	padding: 20px;
}
</style>