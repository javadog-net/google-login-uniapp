<template>
	<view class="content">
		<image class="logo" :src="pic"/>
		<view class="title">{{title}}</view>
		<view class="title">{{email}}</view>
    <view class="title">
      <button type="primary" style="margin-top: 10px" @click="auth">谷歌授权登录</button>
    </view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				pic: '/static/logo.png',
				email: ''
			}
		},
		onLoad() {
			this.login();
		},
		methods: {
			login() {
				const urlParams = new URLSearchParams(window.location.search);
				const code = urlParams.get('code');
				if (!code) {
					return
				}
				const tokenEndpoint = 'https://oauth2.googleapis.com/token';
				const requestBody = new URLSearchParams();
				requestBody.append('code', code);
				requestBody.append('client_id',
					'你的client_id');
				requestBody.append('client_secret', '你的client_secret');
				requestBody.append('redirect_uri', 'http://localhost:8080');
				requestBody.append('grant_type', 'authorization_code'); //这些参数在之前配置的有，看前面的代码

				fetch(tokenEndpoint, {
						method: 'POST',
						headers: {
							'Content-Type': 'application/x-www-form-urlencoded',
						},
						body: requestBody
					})
					.then(response => response.json())
					.then(data => {
						const accessToken = data.access_token; //打印data可以获得token令牌的信息


						fetch('https://www.googleapis.com/oauth2/v2/userinfo', {
								headers: {
									Authorization: `Bearer ${accessToken}`
								}
							})
							.then(response => response.json())
							.then(userInfo => {
								this.title = userInfo.name
								this.pic = userInfo.picture
								this.email = userInfo.email
								console.log(userInfo)
							})

					})
			},
			auth() {
				window.clientId =
					'你的client_id'; // 你的 Google OAuth 客户端 ID
				window.redirectUri = 'http://localhost:8080'; // 重定向 URI
				window.scope = 'email profile'; // 请求的权限范围，可以根据需求修改
				window.state = ''; // 用于防止跨站请求伪造（CSRF）攻击，可以不设置，可以随心设置
				window.responseType = 'code'; // 授权响应类型，表示要求返回授权码
				window.clientSecret = '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'; //Google OAuth 客户端密钥（不是api密钥）
				window.grantType = 'authorization_code';
				//&prompt=login 把它加到window.authUrl的末尾可以让用户每次都需要重新输入账号和密码
				window.authUrl =
					`https://accounts.google.com/o/oauth2/v2/auth?client_id=${window.clientId}&redirect_uri=${window.redirectUri}&scope=${window.scope}&state=${window.state}&response_type=${window.responseType}`;
				//点击Google登录  执行这个方法进行跳转
				window.location.href = window.authUrl
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
    margin: 200 rpx auto 50 rpx;
    margin-top: 40rpx;
  }

	.title {
		font-size: 36rpx;
		color: #8f8f94;
    margin-top: 20rpx;
	}
</style>
