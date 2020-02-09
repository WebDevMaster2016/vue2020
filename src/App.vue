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
				<b-button variant="primary"
				          @click="showModal"
				          :disabled="!formReady"
				>
					Send Data
				</b-button>
			</form>
			<div v-else>
				<hr>
				<h1>All done!</h1>
			</div>
			<b-modal ref="my-modal" title="Confirm data">
				<table class="table table-bordered">
					<tr v-for="(field) in info">
						<td>{{ field.name }}</td>
						<td>{{ field.value }}</td>
					</tr>
				</table>
				<template v-slot:modal-footer>
					<b-button variant="secondary"
					          @click="hideModal"
					>Cancel</b-button>
					<b-button variant="primary"
					          @click="showResult"
					>0K</b-button>
				</template>
			</b-modal>
		</div>
	</b-container>
</template>

<script>
	import AppField from './components/Field.vue';
	import {BProgress, BContainer, BModal, BButton } from 'bootstrap-vue'
	
	export default {
		components: {
			AppField,
			BProgress,
			BContainer,
			BModal,
            BButton
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
			},
            showModal() {
                this.$refs['my-modal'].show()
            },
            hideModal() {
                this.$refs['my-modal'].hide()
            },
            showResult() {
			    this.hideModal();
			    this.formDone = true;
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