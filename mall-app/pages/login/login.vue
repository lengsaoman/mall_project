<template>
	<view class="container">
		<view class="left-bottom-sign"></view>
		<view class="back-btn yticon icon-zuojiantou-up" @click="navBack"></view>
		<view class="right-top-sign"></view>
		<view class="wrapper">

			<!-- H5、Android、IOS登录授权界面 -->
			<!-- #ifndef MP -->
			<view class="left-top-sign">LOGIN</view>
			<view class="welcome">欢迎回来！</view>
			<view class="input-content">
				<view class="input-item" style="position: relative;">
					<text class="tit">手机号码</text>
					<input :value="phoneNumber" placeholder="请输入手机号码" maxlength="11" data-key="phoneNumber"
						@input="inputChange" />
					<!-- 					<button :disabled="!isCorretPhoneNumber" class="sms-code-btn"
						:class="{correct_phone_number:isCorretPhoneNumber}" @click.prevent="getSmsCode">
						{{countdown>0 ? `(${countdown}s)已发送` : '获取验证码'}}
					</button> -->
				</view>

				<view class="input-item">
					<text class="tit">密码</text>
					<input :value="password" placeholder="请输入密码" placeholder-class="input-empty" maxlength="20"
						data-key="password" @input="inputChange" />
				</view>

				<view class="input-item">
					<text class="tit">用户名</text>
					<input :value="username" placeholder="请输入用户名" placeholder-class="input-empty" maxlength="20"
						data-key="username" @input="inputChange" />
				</view>
			</view>
			<button class="confirm-btn" @click="toLogin" :disabled="logining">登录</button>
			<!-- 			<view class="tip">
				默认手机号码/验证码: 17621590365/666666
			</view> -->
			<!-- #endif -->
		</view>
		<view class="register-section">
			还没有账号?
			<text @click="toRegist">马上注册</text>
		</view>
	</view>
</template>

<script>
	import {
		mapMutations
	} from 'vuex';

	// import {
	// 	getUserInfo,
	// 	login
	// } from '@/api/user.js';
	import {
		log
	} from 'util';

	export default {
		data() {
			return {
				phoneNumber: '1234567890',
				password: '123456',
				username: '杨宗',
				logining: false,
				countdown: 0,
				timer: null,
				code: undefined
			};
		},
		computed: {
			// 手机号码验证
			isCorretPhoneNumber() {
				return /^1[3456789]\d{9}$/.test(this.mobile);
			}
		},
		onLoad() {

		},
		methods: {
			...mapMutations(['SET_HAS_LOGIN','SET_NICKNAME','SET_AVATAR','SET_BALANCE','SET_MEMBERID']),
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},
			toRegist() {
				uni.navigateTo({
					url: '/pages/signup/signup'
				})
			},

			//  #ifndef MP

			async toLogin() {
				this.logining = true;
				this.$store.dispatch('user/login', {
					username: this.username,
					password: this.password,
					phoneNumber: this.phoneNumber
				}).then(res => {
					this.$store.dispatch('user/getUserInfo', this.username);
					console.log("登录成功")
					const pages = getCurrentPages();
					if (pages.length > 1) {
						uni.navigateBack()
					} else {
						console.log("跳转首页")
						// 跳转首页
						uni.switchTab({
							url: '/pages/index/index'
						});
					}
				}).catch(() => {
					this.logining = false;
				});
			},

			// async toLogin() {
			// 	this.logining = true;
			// 	let result1;
			// 	let result2;
			// 	try {
			// 		result1 = await login(this.username, this.password, this.phoneNumber)
			// 		uni.showToast({
			// 			title: result1.message
			// 		})
			// 		if (result1.success) {
			// 			this.$store.state.hasLogin = true
			// 			try {
			// 				result2 = await getUserInfo(this.username)
			// 				console.log(result2.data)
			// 				if (result2.success) {
			// 					this.SET_NICKNAME(this.username)
			// 				}
			// 			} catch (err) {}

			// 			this.logining = false;
			// 		}
			// 	} catch (err) {}
			// }
			// #endif
		}
	};
</script>

<style lang="scss">
	page {
		background: #fff;
	}

	.container {
		padding-top: 115px;
		position: relative;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		background: #fff;
	}

	.wrapper {
		position: relative;
		z-index: 90;
		background: #fff;
		padding-bottom: 40upx;
	}

	.back-btn {
		position: absolute;
		left: 40upx;
		z-index: 9999;
		padding-top: var(--status-bar-height);
		top: 40upx;
		font-size: 40upx;
		color: $font-color-dark;
	}

	.left-top-sign {
		font-size: 120upx;
		color: $page-color-base;
		position: relative;
		left: -16upx;
	}

	.right-top-sign {
		position: absolute;
		top: 80upx;
		right: -30upx;
		z-index: 95;

		&:before,
		&:after {
			display: block;
			content: '';
			width: 400upx;
			height: 80upx;
			background: #b4f3e2;
		}

		&:before {
			transform: rotate(50deg);
			border-radius: 0 50px 0 0;
		}

		&:after {
			position: absolute;
			right: -198upx;
			top: 0;
			transform: rotate(-50deg);
			border-radius: 50px 0 0 0;
			/* background: pink; */
		}
	}

	.left-bottom-sign {
		position: absolute;
		left: -270upx;
		bottom: -320upx;
		border: 100upx solid #d0d1fd;
		border-radius: 50%;
		padding: 180upx;
	}

	.welcome {
		position: relative;
		left: 50upx;
		top: -90upx;
		font-size: 46upx;
		color: #555;
		text-shadow: 1px 0px 1px rgba(0, 0, 0, 0.3);
	}

	.input-content {
		padding: 0 60upx;
	}

	.input-item {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: center;
		padding: 0 30upx;
		background: $page-color-light;
		height: 120upx;
		border-radius: 4px;
		margin-bottom: 50upx;

		&:last-child {
			margin-bottom: 0;
		}

		.tit {
			height: 50upx;
			line-height: 56upx;
			font-size: $font-sm + 2upx;
			color: $font-color-base;
		}

		input {
			height: 60upx;
			font-size: $font-base + 2upx;
			color: $font-color-dark;
			width: 100%;
		}
	}

	.sms-code-btn {
		position: absolute;
		right: 0;
		border: none;
		color: #ccc;
		background: transparent;
		font-size: 14px;
	}

	.correct_phone_number {
		color: #000;
	}


	.confirm-btn {
		width: 630upx;
		height: 76upx;
		line-height: 76upx;
		border-radius: 50px;
		margin-top: 150upx;
		background: $uni-color-primary;
		color: #fff;
		font-size: $font-lg;

		&:after {
			border-radius: 100px;
		}
	}

	.forget-section {
		font-size: $font-sm + 2upx;
		color: $font-color-spec;
		text-align: center;
		margin-top: 40upx;
	}

	.register-section {
		position: absolute;
		left: 0;
		bottom: 50upx;
		width: 100%;
		font-size: $font-sm + 2upx;
		color: $font-color-base;
		text-align: center;

		text {
			color: $font-color-spec;
			margin-left: 10upx;
		}
	}

	.tip {
		margin-top: 60upx;
		font-size: $font-base - 4upx;
		color: $font-color-dark;
		text-align: center;
		font-family: yticon;
		font-weight: bold;
	}
</style>