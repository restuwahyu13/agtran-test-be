<template>
	<div>
		<form @submit.prevent="socialLogin">
			<div class="form-group">
				<button type="submit" class="btn btn-dark form-control mt-2">Login With Google</button>
			</div>
		</form>
	</div>
</template>

<script>
import { setAuth, isAuthLocal } from '../utils/auth'

export default {
	name: 'GoogleAuth',
	mounted() {
		this.fetchAuth()
	},
	methods: {
		socialLogin() {
			window.location.href = '/auth/google'
			window.localStorage.setItem('loading', true)
		},
		fetchAuth() {
			if (window.localStorage.getItem('loading') == 'true') {
				this.$http.get(`/auth/google/response`).then((res) => {
					if (res.data) {
						setAuth('social', res)
						this.checkAuth()
					}
				})
			}
		},
		checkAuth() {
			if (isAuthLocal()) {
				this.$router.go('/')
			} else {
				setTimeout(() => this.$router.push('/login'), 5000)
			}
		}
	}
}
</script>

<style scoped></style>
