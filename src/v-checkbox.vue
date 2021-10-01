<template>
	<div class="v-checkbox-container"
		:class="classes"
		:style="[setStyleHeight, setStyleContainerMargin]"
	>
		<!-- Native checkbox -->
		<input type="checkbox"
			class="v-checkbox-hidden v-checkbox-container__v-checkbox-hidden jcf-ignore"
			:style="setStyleHeight"
			:checked="value"
			:disabled="disabled"
			@change="$emit('input', !value)"
			@keypress.enter="$emit('input', !value)"
		>
		
		<!-- Custom checkbox -->
		<div class="v-checkbox"
			:class="[setClassViewTypeCheckBox, setClassViewDetailCheckBox, setClassDisabledCheckBox]"
			:style="setStyleDimensionsCheckBox"
		>
			<slot>
				<div class="v-checkbox-image"
					:class="setClassViewTypeImage"
				></div>
			</slot>
		</div>

		<!-- Label -->
		<span class="v-checkbox-label v-checkbox-container__v-checkbox-label"
				:class="[setClassLabelPosition, setClassContainsLabel]"
			>
			<template v-if="Array.isArray(label) && label.length">
				<template v-for="(el, i) of label">
					<a v-if="'link' in el && el.link"
						class="v-checkbox-target"
						:href="el.link"
					>
						{{ el.text }}
					</a>
					<template v-else>
						{{ el.text }}
					</template>
				</template>
			</template>
			<template v-else>
					{{ label }}
			</template>
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
			type: [String, Array],
			default: ''
		},
		labelPosition: {
			type: String,
			default: 'right'
		},
		disabled: {
			type: Boolean,
			default: false
		},
		viewType: {
			type: String,
			default: 'square'
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
	data: () => ({
		specificWidth: {
			square: 24,
			rectangle: 52
		},
		specificHeight: {
			square: 24,
			rectangle: 32
		},
	}),
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
			const width = 'width' in this.$options.propsData
				? this.width
				: this.specificWidth[this.viewType]
			const height = 'height' in this.$options.propsData
				? this.height
				: this.specificHeight[this.viewType]

			return {
				minWidth: `${+width}px`,
				minHeight: `${+height}px`,
				width: `${+width}px`,
				height: `${+height}px`
			}
		},
		setClassLabelPosition() {
			if (this.labelPosition === 'left' && (this.label || this.label.length)) {
				return 'v-checkbox-label--label-left'
			}
		},
		setClassContainsLabel() {
			if (Array.isArray(this.label) && !this.label.length
				|| typeof this.label === 'string' && !this.label
			) {
				return 'v-checkbox-label--label-empty'
			}
		},
		setClassViewDetailCheckBox() {
			if (!Object.keys(this.$slots).length) {
				return this.value
					? `v-checkbox-${this.viewType}--yes`
					: `v-checkbox-${this.viewType}--no`
			}
		},
		setClassViewTypeCheckBox() {
			return `v-checkbox-${this.viewType}`
		},
		setClassViewTypeImage() {
			return `v-checkbox-image-${this.viewType}`
		},
		setClassDisabledCheckBox() {
			return this.disabled ? `v-checkbox-${this.viewType}--disabled` : null
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
			font-size: 14px;
			margin: 0 0 0 8px;
			color: #1F1F33;
		}
	}

	// .v-checkbox-hidden {
	// 	&:focus + .v-checkbox-square {
	// 		border: 2px solid #9494a1;
	// 	}
	// }

	.v-checkbox-container:hover {
		.v-checkbox {
			&-square {
				&--yes {
					border: 2px solid #d0d0e2;
					background-color: #d0d0e2;

					// .v-checkbox-image {
					// 	animation: in-checkbox .2s ease-out forwards;
					// 	@keyframes in-checkbox {
					// 		100% { transform: rotateZ(-10deg) }
					// 	}
					// }
				}
				&--no {
					border: 2px solid #d0d0e2;
					background: #eeedf7;
				}
			}

			&-rectangle {
				&--yes {
					.v-checkbox-image {
						&-rectangle {
							box-shadow: -1px 2px 2px rgba(233, 230, 230, 1);
						}
					}
				}
				&--no {
					.v-checkbox-image {
						&-rectangle {
							box-shadow: 1px 2px 2px rgba(4, 14, 47, .4);
						}
					}
				}
				&--disabled {
					.v-checkbox-image {
						&-rectangle {
							box-shadow: -1px 2px 2px rgba(4, 14, 47, .2);
						}
					}
				}
			}

			&--disabled {
				opacity: 1;
			}
		}
	}

	.v-checkbox {
		order: 1;
		transition: .1s;
		
		&-square {
			display: flex;
			justify-content: center;
			align-items: center;
			border-radius: 6px;

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
			&--disabled {
				border: 2px solid #c6c6d4;
				background: #c6c6d4;
				opacity: .5;
			}
			& > * {
				width: 20px;
			}
		}
		&-rectangle {
			border-radius: 24px;
			position: relative;

			&--yes {
				background-color: #1f1f33;
				border: 2px solid #1f1f33;

				.v-checkbox-image {
					animation: yes-rectangle-toggle .2s forwards;
					@keyframes yes-rectangle-toggle {
						100% { left: calc(100% - 28px) }
					}
				}
			}
			&--no {
				background-color: #c4c4c4;
				border: 2px solid #c4c4c4;
	
				.v-checkbox-image {
					animation: no-rectangle-toggle .2s forwards;
					@keyframes no-rectangle-toggle {
						0% { left: calc(100% - 28px) }
						100% { left: 0px }
					}
				}
			}
			&--disabled {
				background: #c6c6d4;
				border: 2px solid #c6c6d4;
				opacity: .5;
			}
			& > * {
				width: 28px;
				height: 28px;
			}
		}
	}

	.v-checkbox-image {
		position: relative;

		&-square {
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

		&-rectangle {
			width: 28px;
			height: 28px;
			border-radius: 50%;
			background: #fff;
			left: 0;
			top: 0;
		}

	}

	.v-checkbox-label {
		font-size: 12px;
		order: 2;

		&--label-left {
			margin: 0 8px 0 0;
			text-align: right;
			order: 0;
		}
		&--label-empty {
			margin: 0;
		}
	}

	.v-checkbox-target {
		position: relative;
		z-index: 120;
		color: #A2A2B9;
		outline: none;
		text-decoration: none;
		border-bottom: 1px solid #A2A2B9;
		transition: .2s;

		&:focus {
			border-bottom: 1px solid #727281;
		}
		&:hover {
			color: #8080bd;
			border-bottom: 1px solid transparent;
		}
	}
</style>