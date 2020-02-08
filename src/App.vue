<template>
	<b-container fluid>
		<div class="sample">
			<form v-if="!formDone">
				<b-progress :value="fieldsDone" :max="info.length"></b-progress>
				<div>
					<app-field v-for="(field, i) in info"
					           :key="i"
					           :name="field.name"
					           :value="field.value"
					           :pattern="field.pattern"
					           @change="onInput(i, $event)"
					>
					</app-field>
				</div>
				<button class="btn btn-primary"
						type="button"
				        :disabled="!formReady"
				        @click="formDone = !formDone"
				>
					Send Data
				</button>
			</form>
			<div v-else>
				<table class="table table-bordered">
					<tr v-for="(field) in info">
						<td>{{ field.name }}</td>
						<td>{{ field.value }}</td>
					</tr>
				</table>
			</div>
		</div>
	</b-container>
</template>

<script>
	import AppField from './components/Field.vue';
	import {BProgress, BContainer } from 'bootstrap-vue'
	
	export default {
		components: {
			AppField,
			BProgress,
			BContainer
		},
		data() {
			return {
				info: [
					{
						name: 'Name',
						value: 'Dmitry',
						pattern: /^[a-zA-Z ]{2,30}$/
					},
					{
						name: 'Phone',
						value: '',
						pattern: /^[0-9]{7,14}$/
					},
					{
						name: 'Email',
						value: '',
						pattern: /.+/
					},
					{
						name: 'Some Field 1',
						value: '',
						pattern: /.+/
					},
					{
						name: 'Some Field 2',
						value: '',
						pattern: /.+/
					}
				],
				formDone: false
			}
		},
		computed: {
			fieldsDone() {
				return this.info.reduce((total, field) => {
					return total + (field.valid ? 1 : 0);
				}, 0);
			},
			formReady() {
				return this.fieldsDone === this.info.length;
			}
		},
		methods: {
			onInput(ind, payload) {
				let field = this.info[ind];
				field.value = payload.value;
				field.valid = payload.valid;
			}
		},
		created() {
			this.info.forEach(field => {
				this.$set(field, 'valid', false);
			});
		},
	}
</script>

<style scoped>
	.container-fluid {
		padding: 15px;
		max-width: 900px;
	}

	.progress {
		margin-bottom: 20px;
	}
</style>