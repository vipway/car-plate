<template>
	<view>
		<view @click="show" :class="valueClass">{{displayValue}}</view>
		<view class="carno" v-if="shown" @click="cancel">
			<view>
				<view class="carno-digit" v-for="(digit, index) in carnoArr" :key="index" :class="{'current': current === index}"
				 @click="onClickDigit(i)">{{digit}}</view>
			</view>
			<view class="err" v-if="errMsg">{{errMsg}}</view>
		</view>
		<view class="keyboard" v-if="shown">
			<view style="position: relative">
				<view class="header">
					<text class="clearNo" @click="clear">清除</text>
					<text class="confirm" @click="confirm">完成</text>
				</view>
				<view class="line" v-if="current === 0" :class="line.size" v-for="(line, index) in carTxt" :key="index">
					<view class="key" :class="{'delete': key === 'delete'}" v-for="(key, index) in line.name" :key="index" @click="onClickKey(key, line.type)">
						<image class="image" v-if="key === 'delete'" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAClUlEQVRoQ+1Y/XXbIBC/EwMknaDZIAQPUHeCphM0mSDuBOkGbSeIvUE7QdwBIMoG7QbOAHB9l4f8sKxng7Ckpxfpr7wYjt/HcRwgjPzDkeOHicDQDk4OTA5kKjClUKaA2dPfrgNSynMhxCcimiPiRbaUhwNsAGBtrV2VZcl/b79WDiil7gFggYjnHQPfCU9Ef51zVyGJZAJKqQdEvKkiE9EfVqdLIt7hawA4A4CV1nq7fhKBGviVtXZRt7QrIkqpH4h4x4IZY+bVOtEElFKcMt/9xB0VugIdxlVKfUPE+1YElFI3iPgwFHhetzWBEDwRPTvn5n2lTbYDUkophHjiQEOCb+UAgy+K4pFLJRH9c87JVOX5rCiK4omrCBHdGmOWTftlNpvx/78AwC+t9eemMUkpFIIHgBdrLadNmbpZpZRzIcRjUHL3SATgediL1rrxbIkmEKqWA74CXQPIqbglcei3ulhRBHx7wIpJDmCt5VMvWfn64k1AEZFrOafN63coxaL3QMUyJmBqOtVJhPOPgU8hsEbED0T00xizSAV5bHwTiRjwb4rA63E92hTyFYjT6HKUm5hBexIlIr4noo1z7mNOJeq1jFab0LcQ3OOf5ZBIPch4LWPMu+yT2DvBfVBFYu8mdKziNLgZ00r81lrzxWXvizrI6rPCZg4ASmstp9POnTSGyCnGtCLg6294FxiMRGsCdRJEtDTG3J5C1ZQYWQQ8ie2VcggS2QSYRFgSmYRz7mtfeyL7Ul/Z3dDPrImo02cVALionnLqfVr0q0SYr95Obvb4naa3r+lm2IpAUOOvua8nok6fFhFxwy4755YneVrsTfKIhVo7EBG7lyETgV5kPrDI5MDkQKYCUwplCpg9ffQO/Aeg2lZPjSEuaAAAAABJRU5ErkJggg==" />
						<text v-else>{{key}}</text>
					</view>
				</view>

				<view class="line" v-if="current > 0" :class="line.size" v-for="(line, index) in numAlphaText" :key="index">
					<view class="key" :class="{'delete': key === 'delete', 'disabled': current === 1 && line.type === 'number'}" v-for="(key, index) in line.name"
					 :key="index" @click="onClickKey(key, line.type)">
						<image class="image" v-if="key === 'delete'" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAClUlEQVRoQ+1Y/XXbIBC/EwMknaDZIAQPUHeCphM0mSDuBOkGbSeIvUE7QdwBIMoG7QbOAHB9l4f8sKxng7Ckpxfpr7wYjt/HcRwgjPzDkeOHicDQDk4OTA5kKjClUKaA2dPfrgNSynMhxCcimiPiRbaUhwNsAGBtrV2VZcl/b79WDiil7gFggYjnHQPfCU9Ef51zVyGJZAJKqQdEvKkiE9EfVqdLIt7hawA4A4CV1nq7fhKBGviVtXZRt7QrIkqpH4h4x4IZY+bVOtEElFKcMt/9xB0VugIdxlVKfUPE+1YElFI3iPgwFHhetzWBEDwRPTvn5n2lTbYDUkophHjiQEOCb+UAgy+K4pFLJRH9c87JVOX5rCiK4omrCBHdGmOWTftlNpvx/78AwC+t9eemMUkpFIIHgBdrLadNmbpZpZRzIcRjUHL3SATgediL1rrxbIkmEKqWA74CXQPIqbglcei3ulhRBHx7wIpJDmCt5VMvWfn64k1AEZFrOafN63coxaL3QMUyJmBqOtVJhPOPgU8hsEbED0T00xizSAV5bHwTiRjwb4rA63E92hTyFYjT6HKUm5hBexIlIr4noo1z7mNOJeq1jFab0LcQ3OOf5ZBIPch4LWPMu+yT2DvBfVBFYu8mdKziNLgZ00r81lrzxWXvizrI6rPCZg4ASmstp9POnTSGyCnGtCLg6294FxiMRGsCdRJEtDTG3J5C1ZQYWQQ8ie2VcggS2QSYRFgSmYRz7mtfeyL7Ul/Z3dDPrImo02cVALionnLqfVr0q0SYr95Obvb4naa3r+lm2IpAUOOvua8nok6fFhFxwy4755YneVrsTfKIhVo7EBG7lyETgV5kPrDI5MDkQKYCUwplCpg9ffQO/Aeg2lZPjSEuaAAAAABJRU5ErkJggg==" />
						<text v-else>{{key}}</text>
					</view>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	import Vue from 'vue'
	export default {
		name: 'car-plate',
		data() {
			return {
				shown: false,
				current: 0,
				valueClass: 'empty',
				carnoArr: ['', '', '', '', '', '', '', ''],
				errMsg: '',
				carTxt: [{
					name: ['京', '津', '沪', '渝', '冀', '晋', '辽', '吉', '黑', '苏'],
					size: 'normal',
					type: 'province'
				}, {
					name: ['浙', '皖', '闽', '赣', '鲁', '豫', '鄂', '湘', '粤'],
					size: 'normal',
					type: 'province'
				}, {
					name: ['琼', '川', '贵', '云', '陕', '甘', '青', '蒙'],
					size: 'normal',
					type: 'province'
				}, {
					name: ['桂', '宁', '藏', '新', '港', '澳', '学', 'delete'],
					size: 'normal',
					type: 'province'
				}],
				numAlphaText: [{
					name: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'],
					size: 'normal',
					type: 'number'
				}, {
					name: ['Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P'],
					size: 'normal',
					type: 'alpha'
				}, {
					name: ['A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L'],
					size: 'normal',
					type: 'alpha'
				}, {
					name: ['Z', 'X', 'C', 'V', 'B', 'N', 'M', 'delete'],
					size: 'normal',
					type: 'alpha'
				}]
			}
		},
		props: {
			value: [String] // 初始值
		},
		computed: {
			displayValue() {
				const disp = []
				const valueArr = this.value ? this.value.split('') : []
				valueArr.forEach((f, i) => {
					disp.push(f)
					if (i === 1) {
						disp.push(' ')
					}
				})
				if (disp.length >= 7) {
					// this.valueClass = ''
					return disp.join('')
				} else {
					// this.valueClass = 'empty'
					return '请点击录入车牌号'
				}
			}
		},
		methods: {
			show() {
				document.activeElement.blur()
				let valueArr = this.value ? this.value.split('') : []
				for (let i = 0; i <= 7; i++) {
					if (valueArr[i]) {
						this.carnoArr[i] = valueArr[i]
					} else {
						this.carnoArr[i] = ''
					}
				}
				const scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop
				window.scrollTo(0, scrollTop + 1)
				this.current = 0
				this.shown = true
			},
			onClickDigit() {
				this.current = c
			},
			onClickKey(key, type) {
				this.errMsg = ''
				if (key === 'delete') {
					this.carnoArr[this.current] = ''
					this.carnoArr = Array.from(this.carnoArr)
					if (this.current > 0) {
						this.current--
					}
				} else {
					this.carnoArr[this.current] = key
					this.carnoArr = Array.from(this.carnoArr)
					if (this.current < this.carnoArr.length - 1) {
						this.current++
					}
				}
			},
			confirm() {
				let newValue = []
				this.carnoArr.forEach((digit, index) => {
					if (digit !== '') {
						newValue.push(digit)
					}
				})
				console.log(newValue)
				if (newValue.length > 0 && newValue.length < 7) {
					this.errMsg = '请填写完整车牌号'
				} else {
					this.$emit('input', newValue.join(''))
					this.shown = false
				}
			},
			clear() {
				this.carnoArr = ['', '', '', '', '', '', '', '']
				this.current = 0
				// this.shown = false
			},
			cancel() {
				this.shown = false
			}
		}
	}
</script>

<style>
	.header {
		height: 30px;
		line-height: 30px;
		background: #ffffff;
		border-bottom: 1px solid #efefef;
		padding: 0 10px;
		margin-bottom: 10px;
		font-size: 14px;
	}

	.header .clearNo {
		float: left;
		color: #666666;
	}

	.header .confirm {
		float: right;
		color: #4983ff;
	}

	.carno {
		position: fixed;
		top: 0px;
		left: 0px;
		width: 100%;
		height: 100%;
		padding-top: 30%;
		/* margin-left: -0.5rem; */
		text-align: center;
		background-color: rgba(0, 0, 0, 0.7);
		vertical-align: baseline;
	}

	.carno .carno-digit {
		display: inline-block;
		width: 8%;
		height: 40px;
		line-height: 40px;
		margin-right: 2vw;
		color: #ffffff;
		font-size: 24px;
		border-bottom: 2px solid #ffffff;
	}

	.carno .carno-digit:nth-child(2) {
		margin-right: 20px;
	}

	.carno .carno-digit.current {
		color: #4983ff;
		border-color: #4983ff;
		animation: cursor 1s infinite ease-in-out;
	}

	.errMsg {
		color: #ffffff;
	}

	.keyboard {
		position: fixed;
		bottom: 0px;
		left: 0px;
		width: 100%;
		height: auto;
		background: rgb(238, 238, 238);
		z-index: 1000;
		padding-bottom: 10px;
	}

	.line {
		text-align: center;
	}

	.key {
		display: inline-block;
		background: #ffffff;
		border-radius: 5px;
		margin: 0.8vw;
		padding: 0;
		height: 20px;
		line-height: 20px;
		text-align: center;
		box-shadow: 0px 0px 2px #B2B5BA;
		width: 20px;
		font-size: 14px;
	}

	.key i {
		font-size: 14px;
	}

	.key:active {
		background: #B2B5BA;
		color: #ffffff;
	}

	.key.disabled {
		background: #dddddd;
	}

	.key.delete {
		background: #D1D2D7;
	}

	.key.delete:active {
		background: #F8F8F8;
	}

	.key.delete .image {
		height: 48upx;
		width: 48upx;
		vertical-align: middle;
	}

	.line .big .key {
		height: 10vw;
		line-height: 10vw;
		width: 10vw;
	}

	.line.normal .key {
		height: 10vw;
		line-height: 10vw;
		width: 8vw;
	}

	.line.normal .key i {
		font-size: 12px;
	}

	.line.mini .key {
		height: 6vw;
		width: 6vw;
	}

	.err {
		margin-top: 15vh;
		color: #fff;
	}

	.animated {
		animation-duration: 0.5s;
		animation-fill-mode: both;
	}

	@media (prefers-reduced-motion) {
		.animated {
			animation: unset !important;
			transition: none !important;
		}
	}

	@keyframes cursor {
		0% {
			border-color: #4983ff;
			color: #4983ff;
		}

		50% {
			border-color: #ffffff;
			color: #ffffff;
		}

		100% {
			border-color: #4983ff;
			color: #4983ff;
		}
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
		}

		to {
			opacity: 1;
		}
	}

	.fadeIn {
		animation-name: fadeIn;
	}

	@keyframes fadeOut {
		from {
			opacity: 1;
		}

		to {
			opacity: 0;
		}
	}

	.fadeOut {
		animation-name: fadeOut;
	}

	@keyframes slideInUp {
		from {
			transform: translate3d(0, 100%, 0);
			visibility: visible;
		}

		to {
			transform: translate3d(0, 0, 0);
		}
	}

	.slideInUp {
		animation-name: slideInUp;
	}

	@keyframes slideOutDown {
		from {
			transform: translate3d(0, 0, 0);
		}

		to {
			visibility: hidden;
			transform: translate3d(0, 100%, 0);
		}
	}

	.slideOutDown {
		animation-name: slideOutDown;
	}

	.empty {
		color: #A9A9A9;
	}
</style>
