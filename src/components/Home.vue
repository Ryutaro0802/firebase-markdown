<template>
	<div id="home">
		<h1>{{ msg }}</h1>
		<button @click="googleLogin">Googleアカウントでログイン</button>
	</div>
</template>
<script>
export default {
	name: "home",
	data() {
		return {
			msg: "Welcome to markdown"
		}
	},
	methods: {
		googleLogin() {
			firebase.auth().signInWithRedirect(new firebase.auth.GoogleAuthProvider());
		}
	}
}
</script>

