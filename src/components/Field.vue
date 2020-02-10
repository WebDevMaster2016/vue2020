<template>
	<div class="form-group">
		<label class="title">{{ name }}</label>
		<transition name="zoom">
			<v-icon v-show="activated"
					:name="icon.name"
					:class="icon.class"
					:key="icon.name"
			/>
		</transition>
		<input type="text"
		       class="form-control"
		       :value="value"
		       @input="onInputData"
		>
	</div>
</template>

<script>
    import VIcon from 'vue-awesome/components/Icon'
    import 'vue-awesome/icons/check-circle'
    import 'vue-awesome/icons/exclamation-circle'

    export default {
        components: {
			VIcon
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
                if (this.isValid) {
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
	@keyframes zoomIn {
		from {
			opacity: 0;
			transform: scale3d(0.3, 0.3, 0.3);
		}

		50% {
			opacity: 1;
		}
	}

	@keyframes zoomOut {
		from {
			opacity: 1;
		}

		50% {
			opacity: 0;
			transform: scale3d(0.3, 0.3, 0.3);
		}

		to {
			opacity: 0;
		}
	}

	.zoom-enter-active {
		animation: zoomIn .5s;
	}

	.zoom-leave-active {
		animation: zoomOut .5s;
	}

	.title {
        margin-right: 5px;
    }
</style>