<template>
	<div class="v-checkbox-container"
		:style="[setStyleHeight, setStyleContainerMargin]"
	>
		<!-- Native checkbox -->
		<input type="checkbox"
			class="v-checkbox-hidden v-checkbox-container__v-checkbox-hidden"
			:class="classes"
			:style="setStyleHeight"
			:checked="value"
			@change="$emit('input', !value)"
		>
		
		<!-- Custom checkbox -->
		<div class="v-checkbox"
			:class="setClassViewCheckBox"
			:style="setStyleDimensionsCheckBox"
		>
			<slot>
				<div class="v-checkbox-image"></div>
			</slot>
		</div>

		<!-- Label -->
		<span class="v-checkbox-label v-checkbox-container__v-checkbox-label"
			:class="[setClassLabelPosition, setClassContainsLabel]"
		>
			{{ label }}
		</span>
	</div>
</template>

<script>
export default {
	name: 'VCheckBox',
	props: {
		value: {
			type: Boolean,
			default: false,
		},
		label: {
			type: String,
			default: ''
		},
		labelPosition: {
			type: String,
			default: 'right'
		},
		width: {
			type: [String, Number],
			default: 24
		},
		height: {
			type: [String, Number],
			default: 24
		},
		margin: {
			type: [String, Number],
			default: '0 0 25 8'
		},
		classes: {
			type: Array,
			default: () => ([])
		}
	},
	computed: {
		setStyleHeight() {
			return { height: `${+this.height}px` }
		},
		setStyleContainerMargin() {
			return {
				margin: String(this.margin).split(' ').map(p => `${p}px`).join(' ')
			}
		},
		setStyleDimensionsCheckBox() {
			return {
				width: `${+this.width}px`,
				height: `${+this.height}px`
			}
		},
		setClassLabelPosition() {
			if (this.labelPosition === 'left' && this.label) {
				return 'v-checkbox-label--label-left'
			}
		},
		setClassContainsLabel() {
			if (!this.label) {
				return 'v-checkbox-label--label-empty'
			}
		},
		setClassViewCheckBox() {
			if (!Object.keys(this.$slots).length) {
				return this.value
					? 'v-checkbox--yes'
					: 'v-checkbox--no'
			}
		}
	},
}
</script>

<style lang="scss">
	.v-checkbox-container {
		width: fit-content;
		position: relative;
		display: flex;
		align-items: center;

		&__v-checkbox-hidden {
			width: 100%;
			position: absolute;
			top: 0;
			left: 0;
			z-index: 10;
			opacity: 0;
			cursor: pointer;
		}
		&__v-checkbox-label {
			margin: 0 0 0 8px;
		}
	}

	.v-checkbox-container:hover {
		.v-checkbox {
			&--yes {
				border: 2px solid #d0d0e2;
				background-color: #d0d0e2;
			}
			&--no {
				border: 2px solid #d0d0e2;
				background: #eeedf7;
			}
		}

		.v-checkbox-image {
			animation: in-checkbox .2s ease-out forwards;
			@keyframes in-checkbox {
				100% { transform: rotateZ(-10deg) }
			}
		}
	}

	.v-checkbox {
		order: 1;
		border-radius: 6px;
		transition: .1s;
		
		&--yes {
			border: 2px solid #1f1f33;
			background-color: #1f1f33;
		}
		&--no {
			border: 2px solid #eeedf7;

			.v-checkbox-image {
				visibility: hidden;
			}
		}
		& > * {
			width: 100%;
			height: 100%;
			display: block;
		}
	}

	.v-checkbox-image {
		position: relative;

		animation: out-checkbox .2s ease-out forwards;
		@keyframes out-checkbox {
			0% { transform: rotateZ(-10deg) }
		}

		&::before, &::after {
			content: '';
			height: 2px;
			background-color: #fff;
			border-radius: 20%;
			position: absolute;
		}
		&::before {
			width: 7px;
			top: calc(50% + 2px);
			left: 2px;
			transform: translateY(-50%) rotate(45deg)
		}
		&::after {
			width: 14px;
			top: 50%;
			left: 5px;
			transform: translateY(-50%) rotate(-45deg)
		}
	}

	.v-checkbox-label {
		order: 2;

		&--label-left {
			margin: 0 8px 0 0;
			order: 0;
		}
		&--label-empty {
			margin: 0;
		}
	}
</style>