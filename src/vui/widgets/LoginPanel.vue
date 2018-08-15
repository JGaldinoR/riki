<template>
	<div class="container">
		<div class="row">
			<div class="col-md-6 offset-md-3">
				
				<div class="card shadow rounded">
					<div 
						class="card-header" 
						:class="[headerColor, headerTextColor]">
						<h3 class="panel-title">Login</h3>
					</div>
					<div class="card-body">
						<form @submit.prevent="submitLogin">
							<div 
								class="form-group has-feedback" 
								:class="[emailFeedbackClass]">
								<input 
									type="email" 
									class="form-control" 
									id="email"
									ref="txtEmail" 
									placeholder="Email" 
									autofocus
									required
									@input="checkEmailValidation"
									autocomplete="new-email"
								>
								<span :class="[emailIconClass]"></span>
							</div>

							<div 
								class="form-group has-feedback" 
								:class="[passwordFeedbackClass]">
								<input 
									type="password" 
									class="form-control" 
									id="password" 
									ref="txtPassword"
									placeholder="Password"
									:pattern="passwordPattern"
									required
									@input="checkPasswordValidation"
									autocomplete="new-password"
								>
								<span :class="[passwordIconClass]"></span>
								<small id="passwordError" class="bs">
									{{ passwordMessage }}
								</small>
							</div>
							<div 
								v-if="errorMessage" 
								class="text-center text-danger"
							>
								{{ errorMessage }}
							</div>

							<div class="checkbox">
								<label>
									<input 
										type="checkbox" 
										id="chkRemember" 
										v-model="rememberMe"
									> 
									Remember Me
								</label>
								<button 
									class="bs btn btn-primary float-right" 
									:disabled="submitButtonDisabled"
								>
									Sign In
								</button>
							</div>
							
						</form>
					</div>
				</div>

			</div>
		</div>
	</div>
</template>

<script>
export default {
  name: 'LoginComponent',
	props: {
		'headerColor': {
			type: String,
			required: false,
			default: 'bg-primary'
		},
		'headerTextColor': {
			type: String,
			required: false,
			default: 'text-white'
		},
		'errorMessage': {
			type: String,
			required: false,
			default: ''
		},
		'passwordMessage': {
			type: String,
			required: false,
			default: 'Password length between 8 and 12'
		},
		'passwordPattern': {
			type: String,
			required: false,
			default: '.{8,12}'
		}
	},
	data() {
		return {
			emailFeedbackClass: '',
			emailIconClass: '',
			passwordFeedbackClass: '',
			passwordIconClass: '',
			submitButtonDisabled: true,
			rememberMe: false
		}
	},
	mounted() {
		// use js-cookie
		let email = Cookies.get('email')
		let password = Cookies.get('password')
		this.$refs.txtEmail.value = email ? email : ''
		this.$refs.txtPassword.value = password ? password : ''
		
		// Enable button if cookie was set for email
		if (email) {
			this.submitButtonDisabled = false
		}
	},
	methods: {
		checkEmailValidation() {
			if (this.$refs.txtEmail.checkValidity()) {
				this.emailFeedbackClass = 'has-success'
				this.emailIconClass = 'glyphicon glyphicon-ok form-control-feedback'
				if (this.$refs.txtPassword.checkValidity()) {
					this.submitButtonDisabled = false
				}
			}
			else {
				this.emailFeedbackClass = 'has-error'
				this.emailIconClass = 'glyphicon glyphicon-remove form-control-feedback'
				this.submitButtonDisabled = true
			}
		},
		checkPasswordValidation() {
			if (this.$refs.txtPassword.checkValidity()) {
				this.passwordFeedbackClass = 'has-success'
				this.passwordIconClass = 'glyphicon glyphicon-ok form-control-feedback'
				if (this.$refs.txtEmail.checkValidity()) {
					this.submitButtonDisabled = false
				}
			}
			else {
				this.passwordFeedbackClass = 'has-error'
				this.passwordIconClass = 'glyphicon glyphicon-remove form-control-feedback'
				this.submitButtonDisabled = true
			}
		},
		submitLogin() {
			let email = this.$refs.txtEmail.value.trim()
			let password = this.$refs.txtPassword.value.trim()

			// Cookie methods
			if (this.rememberMe) {
				// use js-cookie
				let d = new Date()
				d.setTime(d.getTime() + (180*24*60*60*1000)) // (days*hours*minutes*seconds*miliseconds)
				Cookies.set('email', email, { expires: d.toUTCString, path: '/' });
				Cookies.set('password', password, { expires: d.toUTCString, path: '/' });
			}
			else {
				// use js-cookie
				Cookies.remove('email', { path: '/' })
				Cookies.remove('password', { path: '/' })
			}

			this.$emit('loginCredentials',  { 
				'email': email, 
				'password': password 
			})
		}
	}

}
</script>