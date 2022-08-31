<template>
	<div class="v-checkbox-wrapper"
		:style="setStyleWrapper"
	>
		<div class="v-checkbox-container"
			:class="classes"
			:style="[
				setStyleHeight,
				setStyleContainerPosition,
				{ '--b-color': bColor }
			]"
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
				:class="[
					elClass,
					setClassViewTypeCheckBox,
					setClassViewDetailCheckBox,
					setClassDisabledCheckBox,
				]"
				:style="setStyleRectangleWidth"
			>
				<slot>
					<div class="v-checkbox-image"
						:class="setClassViewTypeImage"
						:style="[{ '--r-size': `${height}px` }, setStyleSizeInnerChecked]"
					></div>
				</slot>
			</div>
	
			<!-- Label -->
			<span class="v-checkbox-label v-checkbox-container__v-checkbox-label"
					:class="[setClassLabelPosition, setClassContainsLabel]"
					:ref="labelRef"
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
							<span v-html="el.text"></span>
						</template>
					</template>
				</template>
				<template v-else>
					<span v-html="label"></span>
				</template>
			</span>
	</div>

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
		checkPosition: {
			type: String,
			default: 'center'
		},
		disabled: {
			type: Boolean,
			default: false
		},
		viewType: {
			type: String,
			default: 'circle'
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
			default: '0 0 0 0'
		},
		elClass: {
			type: String,
			default: ''
		},
		classes: {
			type: Array,
			default: () => ([])
		},
		bColor: {
			type: String,
			default: '#1f1f33'
		}
	},
	data: () => ({
		specificWidth: {
			square: 24,
			circle: 24,
			rectangle: 52
		},
		specificHeight: {
			square: 24,
			circle: 24,
			rectangle: 32
		},
		labelRef: null,
		labelHeight: 0,
	}),
	computed: {
		setStyleHeight() {
			return { height: `${+this.height}px` }
		},
		setStyleWrapper() {
			return {
				height: +this.height < this.labelHeight
					? `${this.labelHeight}px`
					: `${this.height}px`,
				margin: String(this.margin).split(' ').map(p => `${p}px`).join(' '),
			}
		},
		setStyleContainerPosition() {
			const checkMap = {
				top: 'flex-start',
				start: 'flex-start',
				center: 'center',
				bottom: 'flex-end',
				end: 'flex-end'
			}

			return {
				alignItems: checkMap[this.checkPosition] ?? 'center'
			}
		},
		setStyleSizeInnerChecked() {
			return {
				width: `${this.height}px`,
				height: `${this.height}px`
			}
		},
		setStyleRectangleWidth() {
			const width = 'width' in this.$options.propsData
				? this.width
				: this.specificWidth[this.viewType]

			return this.viewType === 'rectangle' ? {
				minWidth: `${+width}px`,
				width: `${+width}px`
			} : null
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
	created() {
		this.labelRef = String(Math.random()).slice(2, 10)
	},
	mounted() {
		this.labelHeight = this.$refs[this.labelRef].offsetHeight
	}
}
</script>

<style lang="scss">
	.v-checkbox-container {
		width: fit-content;
		position: relative;
		display: flex;

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
			&-square, &-circle {
				&--yes {
					opacity: .8;
				}
				&--no {
					border: 2px solid #d0d0e2;
					background: #eeedf7;
				}
			}

			&-rectangle {
				&--yes {
					opacity: .8;
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
			border-radius: 6px;
		}

		&-circle {
			border-radius: 50%;
		}
		
		&-square, &-circle {
			display: flex;
			justify-content: center;
			align-items: center;

			&--yes {
				border: 2px solid var(--b-color);
				background-color: var(--b-color);
			}
			&--no {
				border: 2px solid #eeedf7;
				background-color: #fff;
	
				.v-checkbox-image {
					visibility: hidden;
				}
			}
			&--disabled {
				border: 2px solid #c6c6d4;
				background: #c6c6d4;
				opacity: .5;
			}
		}
		&-rectangle {
			border-radius: 24px;
			position: relative;

			&--yes {
				background-color: var(--b-color);
				border: 2px solid var(--b-color);

				.v-checkbox-image {
					animation: yes-rectangle-toggle .2s forwards;
					@keyframes yes-rectangle-toggle {
						100% { left: calc(100% - var(--r-size)) }
					}
				}
			}
			&--no {
				background-color: #c4c4c4;
				border: 2px solid #c4c4c4;
	
				.v-checkbox-image {
					animation: no-rectangle-toggle .2s forwards;
					@keyframes no-rectangle-toggle {
						0% { left: calc(100% - var(--r-size)) }
						100% { left: 0px }
					}
				}
			}
			&--disabled {
				background: #c6c6d4;
				border: 2px solid #c6c6d4;
				opacity: .5;
			}
		}
	}

	.v-checkbox-image {
		position: relative;

		&-square, &-circle {
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
				left: calc(50% - 8px);
				transform: translateY(-50%) rotate(45deg)
			}
			&::after {
				width: 14px;
				top: 50%;
				left: calc(50% - 5px);
				transform: translateY(-50%) rotate(-45deg)
			}
		}

		&-rectangle {
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