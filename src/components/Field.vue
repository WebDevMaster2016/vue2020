<template>
	<div class="form-group">
		<label class="title">{{ name }}</label>
		<v-icon v-if="activated"
		        :name="icon.name"
		        :class="icon.class"
		/>
		<input type="text"
		       class="form-control"
		       :value="value"
		       @input="onInputData"
		>
	</div>
</template>

<script>
	import Icon from 'vue-awesome/components/Icon'
	import 'vue-awesome/icons/check-circle'
	import 'vue-awesome/icons/exclamation-circle'

	export default {
		components: {
			'v-icon': Icon
		},
		props: {
			name: String,
			value: String,
			pattern: {
				type: RegExp,
				required: true
			}
		},
		data() {
			return {
				activated: this.value != ''
			}
		},
		computed: {
			isValid() {
				return this.pattern.test(this.value);
			},
			icon() {
				if(this.isValid) {
					return {
						name: 'check-circle',
						class: 'text-success'
					}
				} else {
					return {
						name: 'exclamation-circle',
						class: 'text-danger'
					}
				}
			}
		},
		methods: {
			onInputData(e) {
				this.activated = true;
				let value = e.target.value.trim();
				this.$emit('change', {value, valid: this.pattern.test(value)});
			}
		},
		created() {
			if (this.activated) {
				this.$emit('change', {value: this.value, valid: this.isValid});
			}
		}
	}
</script>

<style scoped>
	.title {
		margin-right: 5px;
	}
</style>