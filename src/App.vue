<template>
	<v-app>
		<app-form></app-form>
	</v-app>
</template>

<script>
import Form from './views/form/Form.vue';

export default {
	name: "app",
	components: {
		'app-form': Form,
	},
}
</script>

<style>

</style>