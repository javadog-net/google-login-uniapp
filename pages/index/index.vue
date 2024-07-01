<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view id="google-signin-button" style="margin-top: 20px;"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			init() {
				this.loadGoogleScript();
			},
			/**
			 * 创建并添加一个脚本元素到文档头部，用于加载Google登录的客户端库。
			 * 这是初始化Google Sign-In过程的第一步，它需要在页面加载时异步加载Google的JavaScript库。
			 * 
			 * 通过设置脚本的src属性为Google登录客户端库的URL，script元素将负责下载并执行这个库。
			 * 设置async和defer属性为true，确保脚本可以异步加载，不会阻塞页面的渲染。
			 * 设置onload属性为一个回调函数，当脚本加载完成时，这个回调函数将会被调用，用于初始化Google Sign-In。
			 */
			loadGoogleScript() {
				const script = document.createElement('script');
				script.src = 'https://accounts.google.com/gsi/client';
				script.async = true;
				script.defer = true;
				script.onload = this.initGoogleSignIn;
				document.head.appendChild(script);
			},
			/**
			 * 初始化Google登录按钮并配置登录回调。
			 * 该方法设置了窗口的handleCredentialResponse函数，用于处理Google登录的响应。
			 * 它还初始化了Google账号ID模块，配置了客户端ID、用户界面模式和回调函数。
			 * 最后，它渲染了Google登录按钮，并触发登录提示框。
			 */
			initGoogleSignIn() {
				// 绑定handleCredentialResponse方法到当前上下文，确保this指向正确
				window.handleCredentialResponse = this.handleCredentialResponse.bind(this);
				// 初始化Google账号ID，配置客户端ID、用户体验模式和回调函数
				window.google.accounts.id.initialize({
					client_id: '6764784206-2evu6trfap49bgm6o7ss57dcs65ohdnb.apps.googleusercontent.com',
					ux_mode: 'redirect',
					callback: this.handleCredentialResponse
				});
				// 渲染Google登录按钮，设置按钮主题和大小
				window.google.accounts.id.renderButton(
					document.getElementById('google-signin-button'), {
						theme: 'outline',
						size: 'large'
					}
				);
				// 触发Google登录提示框，引导用户进行登录操作
				window.google.accounts.id.prompt();
			},
			/**
			 * 处理Google登录的响应。
			 * 
			 * 当用户通过Google登录成功后，Google会返回一个认证凭证，本函数负责将这个凭证发送到服务器进行验证。
			 * 验证成功后，会进一步处理登录成功的行为，如设置用户信息等；验证失败则处理登录失败的逻辑。
			 * 
			 * @param {Object} response - Google登录认证的响应对象，包含认证凭证。
			 * @param {string} response.credential - 由Google颁发的认证凭证，用于服务器端验证用户身份。
			 */
			handleCredentialResponse(response) {
				console.log('Encoded JWT ID token: ' + response.credential);
				// 发送请求到后端验证Google返回的ID token
				uni.request({
				    url: '/api/auth/google', 
					method: 'POST',
				    data: {
				        token: response.credential
				    },
				    success: (res) => {
				        console.log(res.data);
				        this.text = 'request success';
				    }
				});
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>