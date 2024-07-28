## 前言

### 🍊缘由

####  狗哥也能玩的很花，谷歌登录手拿把掐

![](https://img.javadog.net/blog/google-login/3b435f269ec649a98a4ce0d68ede5d28.png)


🏀事情起因：

**大家好，我是[JavaDog程序狗](https://mp.weixin.qq.com/s/FjOdxCAPFrJpruhS1YB-MA)**

今天给大家来分享一下**谷歌授权登录，不用后端，前端uniapp直接登录，并获取用户信息**

**你想听的故事**


![](https://img.javadog.net/blog/google-login/f544ef16455046ae91261cf2115efb6c.png)


本狗好友，我称呼他『磊磊』，远在海外的大老板。

磊磊为了拓展海外市场，替国争光去挣老美的刀乐，遂采用本狗项目[SpringBoot+uniapp+uview打造H5+小程序+APP入门学习的聊天小项目](https://mp.weixin.qq.com/s/g7AZOWLgW5vcCahyJDEPKA) 进行二开加入AI元素，包装后进行投资引流。

然而老美都习惯用谷歌登录，所以本狗就协助磊磊进行代码重构，并实现谷歌授权登录，虽然**官网有多种实现方式**，但是目前限制较多(科学限制)，本狗就单独选择**前端直连授权登录获取信息实例**，跟大家进行分享。

本文只是用前端展示简单流程，实际项目一定要配合后端做好安全防护，保证数据隐秘性。


### 🎁如何获取源码

公众号：【JavaDog程序狗】

关注公众号，**发送  “google-login”**，无任何套路即可获得！

![](https://img.javadog.net/blog/google-login/016f82a1ea3c414a91c2145fdd36b3d9.png)

![](https://img.javadog.net/blog/google-login/1199099b92284e7db4de53ee5878e475.png)



### 🎯主要目标

#### 实现3大重点

##### 1. 谷歌授权登录流程

##### 2. 填写谷歌开发者控制台配置

##### 3. uniapp前端授权登录实操

### 🥦目标分析

#### 一. 谷歌授权登录流程

##### 1.用户点击登录按钮：

用户在你的应用上点击“使用Google登录”按钮。

##### 2.重定向到Google的授权服务器：

应用将用户重定向到Google的OAuth 2.0授权端点，附带客户端ID、重定向URI、响应类型（通常是code）、作用域（scope）等参数。

##### 3.用户同意授权：

用户在Google登录页面上输入凭证并同意应用请求的权限。

##### 4.Google重定向回应用：

Google授权服务器将用户重定向回应用的重定向URI，并附带授权码（Authorization Code）。

##### 5.应用服务器交换授权码：

应用服务器向Google的令牌端点发送请求，用授权码交换访问令牌（Access Token）和刷新令牌（Refresh Token）。

##### 6.接收访问令牌：

Google返回访问令牌和可选的刷新令牌，应用服务器使用访问令牌访问Google API。

##### 7.访问受保护资源：

应用服务器使用访问令牌向Google API发送请求，获取用户信息或其他资源。

![](https://img.javadog.net/blog/google-login/e7542a0aab7a488090d81ced6c84aaab.png)

******

#### 二. 填写谷歌开发者控制台配置

任何OAuth2登录流程基本都一致，**谷歌登录也一样，需要在开发者中配置一些必要条件**，就像我们常做的微信授权一样

##### 1. 访问开发者配置官网

[https://console.cloud.google.com/cloud-resource-manager](https://console.cloud.google.com/cloud-resource-manager)

在谷歌管理资源页面，我们要先**创建项目**

![](https://img.javadog.net/blog/google-login/e31114876f9c4d1c89f054e7a18f43b2.png)

##### 2. 创建项目

点击『创建项目』，然后填写**项目名称及位置**，名称必填，位置默认即可。

创建成功后会在列表看到我们刚创建的项目名

![](https://img.javadog.net/blog/google-login/55fc624b173b4238bbb299b945fc9cbe.png)

![](https://img.javadog.net/blog/google-login/ebf9184a054049b69144d34ba67690ba.png)

![](https://img.javadog.net/blog/google-login/a8c01a9199994b8cae8e3e3ad2bb9c7b.png)

##### 3. API和服务配置

要是不好找**API和服务配置**可以直接访问 [https://console.cloud.google.com/welcome](https://console.cloud.google.com/welcome)

在当前项目下，点击『API和服务』，惊醒后续配置

![](https://img.javadog.net/blog/google-login/5ed880d31a214fe49666030d4a689864.png)

##### 4. 创建客户端凭据

在**凭据**菜单中，点击『创建凭据』，选择**OAuth客户端ID**，然后点击配置同意屏幕

 ❓什么是同意屏幕

> 谷歌的“授权配置同意屏幕”（OAuth Consent Screen）是在OAuth 2.0授权流程中向用户展示的一个界面，用于请求用户的许可以访问其谷歌账户中的数据或执行某些操作。
> 当你的应用试图使用OAuth协议从谷歌获取用户数据时，用户会被重定向到这个屏幕上，它会显示应用的一些关键信息，如应用名称、图标、开发者信息以及请求的权限范围。


![](https://img.javadog.net/blog/google-login/232cdafdeb634e16ab9dfc7394465e47.png)

![](https://img.javadog.net/blog/google-login/ed5af12e867a4815b1260f3e42b40ea1.png)

##### 5. OAuth 权限请求页面

创建OAuth权限请求，选择『外部』，后填写应用信息，如必填项**应用名称**、**用户支持电子邮件**、**开发者联系信息**，其余内容可以先选择性忽略，但是**测试用户一定要加上**

![](https://img.javadog.net/blog/google-login/8a7447d7a84047939cb122da82f7ff65.png)

![](https://img.javadog.net/blog/google-login/c54d5c05de1a43a5a8e4be09818ecf80.png)

![](https://img.javadog.net/blog/google-login/9995bd8ae61b40328ed12157ce4ecd57.png)

![](https://img.javadog.net/blog/google-login/7afb152a06eb47c1a0d143fce8949051.png)

![](https://img.javadog.net/blog/google-login/e63a6af4a5c547c69ce610a09d7da20a.png)

##### 6. 返回凭据创建 OAuth 客户端 ID

返回**凭据**创建『客户端 ID』，选择应用类型『Web应用』，进行**已获授权的JavaScript 来源** 和**已获授权的重定向 URI**配置


![](https://img.javadog.net/blog/google-login/de3a731ca17241deb5d32d6de936414c.png)

![](https://img.javadog.net/blog/google-login/c993910f6fef41f38d77e8697e6d53a6.png)

 ❓什么是已获授权的 JavaScript 来源

>  在谷歌授权登录（OAuth 2.0）的上下文中，“已获授权的JavaScript来源”是指那些被允许使用特定OAuth客户端ID来发起身份验证和授权请求的网页或Web应用的URL。
>  当你在Google Cloud Console中注册一个新的OAuth客户端时，你可以指定哪些来源（即域名或完整的URL）可以使用该客户端ID来与Google的身份验证服务进行通信。

 👽人话解释
 
 > 例如，如果狗哥有一个运行在http://locahost:8080的Web应用，并希望用户能够使用他们的Google账户登录，需要在Google Cloud Console中将http://locahost:8080添加为一个"已获授权的JavaScript来源

 ❓什么是已获授权的重定向 URI

> 在谷歌授权登录（OAuth 2.0）流程中，“已获授权的重定向URI”（Authorized Redirect URIs）是指在用户完成授权过程后，Google将控制权和授权结果（通常是授权码或直接的访问令牌）返回给你的应用的具体URL。

 👽人话解释
 
 > 如果你的应用运行在http://locahost:8080，并且你希望接收授权结果的路径是http://locahost:8080，那么你可能需要在Google Cloud Console中注册http://locahost:8080作为你的重定向URI之一

![](https://img.javadog.net/blog/google-login/23e2ca820fac46cfbd4aec2522b435aa.png)

******

#### 三. uniapp前端授权登录实操

通过上方二填写了开发者相关配置，就可以进行前端代码处理，**源码在上方，关注自行领取**

##### 1.代码结构

代码结构非常简单，就是最基础的uniapp初始化页面

![](https://img.javadog.net/blog/google-login/7dedb1e014f84e1092e7a04192e5f988.png)

##### 2.关键代码

- **获取谷歌授权码code**，也就是上方流程图中的第1-4步

![](https://img.javadog.net/blog/google-login/57742388d194429fb94a96b91ae20c48.png)


```js
 auth() {
      // 你的Google OAuth 客户端 ID
      window.clientId =
          '替换成你的Google OAuth 客户端 ID';
      // 重定向 URI
      window.redirectUri = 'http://localhost:8080';
      // 请求的权限范围，可以根据需求修改
      window.scope = 'email profile';
      // 用于防止跨站请求伪造（CSRF）攻击，可以不设置，可以随心设置
      window.state = '';
      // 授权响应类型，表示要求返回授权码
      window.responseType = 'code';
      // 你的Google OAuth 客户端密钥
      window.clientSecret = '替换成你的Google OAuth 客户端密钥';
      window.grantType = 'authorization_code';
      //&prompt=login 把它加到window.authUrl的末尾可以让用户每次都需要重新输入账号和密码
      window.authUrl =
          `https://accounts.google.com/o/oauth2/v2/auth?client_id=${window.clientId}&redirect_uri=${window.redirectUri}&scope=${window.scope}&state=${window.state}&response_type=${window.responseType}`;
      //点击Google登录  执行这个方法进行跳转
      window.location.href = window.authUrl
    },
```

- **授权码code换取访问令牌，并通过令牌获取用户信息**，也就是上方流程图中的第5-7步

![](https://img.javadog.net/blog/google-login/ecb2efb2a38e42dbb51af726dde55466.png)

```js
getUserInfo() {
      const urlParams = new URLSearchParams(window.location.search);
      const code = urlParams.get('code');
      if (!code) {
        return
      }
      // 用授权码交换访问令牌地址
      const tokenEndpoint = 'https://oauth2.googleapis.com/token';
      const requestBody = new URLSearchParams();
      requestBody.append('code', code);
      // 你的 Google OAuth 客户端 ID
      requestBody.append('client_id',
          '替换成你的Google OAuth 客户端 ID');
      // 你的Google OAuth 客户端密钥
      requestBody.append('client_secret', '替换成你的Google OAuth 客户端密钥');
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
            // 获得token令牌的信息
            const accessToken = data.access_token;
            // 调用获取用户信息接口
            fetch('https://www.googleapis.com/oauth2/v2/userinfo', {
              headers: {
                Authorization: `Bearer ${accessToken}`
              }
            })
                .then(response => response.json())
                .then(userInfo => {
                  // 获取用户信息
                  this.title = userInfo.name
                  this.pic = userInfo.picture
                  this.email = userInfo.email
                  console.log(userInfo)
                })

          })
    },
```

- **完整代码**

```html
<template>
  <view class="content">
    <image class="logo" :src="pic"/>
    <view class="title">{{ title }}</view>
    <view class="title">{{ email }}</view>
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
    this.getUserInfo();
  },
  methods: {
    getUserInfo() {
      const urlParams = new URLSearchParams(window.location.search);
      const code = urlParams.get('code');
      if (!code) {
        return
      }
      // 用授权码交换访问令牌地址
      const tokenEndpoint = 'https://oauth2.googleapis.com/token';
      const requestBody = new URLSearchParams();
      requestBody.append('code', code);
      // 你的 Google OAuth 客户端 ID
      requestBody.append('client_id',
          '替换成你的Google OAuth 客户端 ID');
      // 你的Google OAuth 客户端密钥
      requestBody.append('client_secret', '替换成你的Google OAuth 客户端密钥');
      requestBody.append('redirect_uri', 'http://localhost:8080');
      requestBody.append('grant_type', 'authorization_code'); 

      fetch(tokenEndpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: requestBody
      })
          .then(response => response.json())
          .then(data => {
            // 获得token令牌的信息
            const accessToken = data.access_token;
            // 调用获取用户信息接口
            fetch('https://www.googleapis.com/oauth2/v2/userinfo', {
              headers: {
                Authorization: `Bearer ${accessToken}`
              }
            })
                .then(response => response.json())
                .then(userInfo => {
                  // 获取用户信息
                  this.title = userInfo.name
                  this.pic = userInfo.picture
                  this.email = userInfo.email
                  console.log(userInfo)
                })

          })
    },
    auth() {
      // 你的Google OAuth 客户端 ID
      window.clientId =
          '替换成你的Google OAuth 客户端 ID';
      // 重定向 URI
      window.redirectUri = 'http://localhost:8080';
      // 请求的权限范围，可以根据需求修改
      window.scope = 'email profile';
      // 用于防止跨站请求伪造（CSRF）攻击，可以不设置，可以随心设置
      window.state = '';
      // 授权响应类型，表示要求返回授权码
      window.responseType = 'code';
      // 你的Google OAuth 客户端密钥
      window.clientSecret = '替换成你的Google OAuth 客户端密钥';
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
  height: 200 rpx;
  width: 200 rpx;
  margin: 200 rpx auto 50 rpx;
  margin-top: 40 rpx;
}

.title {
  font-size: 36 rpx;
  color: #8f8f94;
  margin-top: 20 rpx;
}
</style>

```

##### 3.启动调试

- 启动uniapp程序，点击**运行** =》**运行到浏览器** =》**Chrome**

![](https://img.javadog.net/blog/google-login/df8fbe59a84744aba0b29f8fcbb81a37.png)

- 查看启动成功页面，默认为http://localhost:8080

本狗处理业务的页面就在http://localhost:8080，因此对应谷歌开发者配置的 **已获授权的JavaScript 来源** 和**已获授权的重定向 URI**两者也填写的http://localhost:8080

![](https://img.javadog.net/blog/google-login/e0b3b6da576a473fbb5c9872d42faa87.png)

##### 4.成果展示

- 访问http://localhost:8080

![](https://img.javadog.net/blog/google-login/e4d877aaf3934f90a9b0f2404926e91c.png)

- 点击登录，跳转谷歌授权登录页面

![](https://img.javadog.net/blog/google-login/31fccb8e431c422a84ddcaa123b0b20e.png)

- 输入密码，点击下一步

![](https://img.javadog.net/blog/google-login/ca8cffc8d7c64b77a44b97057cd9a95e.png)

- 确认授权

![](https://img.javadog.net/blog/google-login/21aa0008ab1b47588666fecf4d407bae.png)

- 授权成功重定向回应用页面，并获取路径中带的授权码，交换访问令牌并调用获取用户信息接口

![](https://img.javadog.net/blog/google-login/3daffbe0786e4c2e8bcf60a34c0231b9.png)

##### 5.异常情况

- 页面点击登录页面没有跳转谷歌

	网络被挡住了
	
![](https://img.javadog.net/blog/google-login/3c3d98ae2b0244dbb91b3873916678d4.png)

- 谷歌授权成功没有跳回应用页面

	检查配置的**已获授权的重定向 URI**地址

![](https://img.javadog.net/blog/google-login/38a38387b2a54aa19f2cc3d8cb63f7a2.png)

- 使用令牌调用获取用户信息接口403

	检查测试用户是否添加

![](https://img.javadog.net/blog/google-login/17a3000043b647edb87bc4e0d57cffa8.png)

## 总结


本文档详细介绍了如何在uniapp项目中集成谷歌授权登录功能，无需后端介入，直接从前端实现。主要分为三个部分：
1. 谷歌授权登录流程：

	阐述了从**用户点击登录按钮到最终获取用户信息的整个OAuth 2.0流程**，包括**重定向至Google授权服务器、用户同意授权、Google重定向回应用、应用服务器交换授权码、接收访问令牌以及访问受保护资源**。
	
2. 谷歌开发者配置：
	
	如何在Google Cloud Console中**创建项目、配置同意屏幕、创建OAuth客户端ID，并设置必要的已获授权的JavaScript来源和重定向URI**，确保前端应用能正确与Google OAuth系统交互。

3. uniapp前端授权登录实操：

	提供了具体的**uniapp代码示例**，包括**如何构造授权请求URL以获取授权码，以及如何使用授权码交换访问令牌，并通过访问令牌获取用户信息**。代码中展示了如何使用fetchAPI与Google OAuth系统通信，获取用户的基本信息如姓名、头像和邮箱


### 🍈猜你想问

####  如何与狗哥联系进行探讨

##### 关注公众号【JavaDog程序狗】

公众号回复【入群】或者【加入】，便可成为【程序员学习交流摸鱼群】的一员，问题随便问，牛逼随便吹，目前**群内已有超过290+个小伙伴啦**！！！

![](https://img.javadog.net/blog/java-lock/2998b2a49f7dc8c8ddd3a345e5694422.png)

##### 2.踩踩狗哥博客

[javadog.net](https://www.javadog.net/)

**里面有狗哥的私密联系方式呦 😘**

>大家可以在里面留言，随意发挥，有问必答

![](https://img.javadog.net/blog/java-lock/7e380641de0de84f8d07a0d85dabe389.png)

******

###  🍯猜你喜欢

####  文章推荐

[【Java】@Transactional事务套着ReentrantLock锁，锁竟然失效超卖了](https://mp.weixin.qq.com/s/QkvbWRleIaNAH_Txo1aUZg)

[【规范】Git分支管理，看看我司是咋整的](https://mp.weixin.qq.com/s/8LRB9k-4EsgSN1lCy5az8A)

[【工具】珍藏免费宝藏工具，不好用你来捶我](https://mp.weixin.qq.com/s/Mj5--CQVaafs_bInMCgUaQ)

[【插件】IDEA这款插件，爱到无法自拔](https://mp.weixin.qq.com/s/IePixEWV5JMG1X2R4mwt6g)

[【规范】看看人家Git提交描述，那叫一个规矩](https://mp.weixin.qq.com/s/EbNWRpSYMdWFv5aUQ2ockw)

[【工具】用nvm管理nodejs版本切换，真香！](https://mp.weixin.qq.com/s/N6qwQpH-oIgFGSWIVDJ-2g)

[【项目实战】SpringBoot+uniapp+uview2打造H5+小程序+APP入门学习的聊天小项目](https://mp.weixin.qq.com/s/g7AZOWLgW5vcCahyJDEPKA)

[【项目实战】SpringBoot+uniapp+uview2打造一个企业黑红名单吐槽小程序](https://mp.weixin.qq.com/s/t_qwF_HvkdW-6TI3sYUHrA)

[【模块分层】还不会SpringBoot项目模块分层？来这手把手教你！](https://mp.weixin.qq.com/s/fpkiNR2tj832a6VxZozwDg)

 
![](https://img.javadog.net/blog/java-lock/5ee9ad70af6c2ec5d3730bd0c15565e6.jpg)