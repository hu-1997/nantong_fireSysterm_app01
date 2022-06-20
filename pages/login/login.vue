<template>
	<view>
		<image class="image-bg" src="../../static/login.jpg"/>
		<form  @submit="submit">
				<view class="form">
					<view class="login_title">欢迎登入</view>
							<view class="inputWrapper">
								<input class="input" type="text" value="" placeholder="请输入用户名" name="userName"/>
							</view>
							<view class="inputWrapper">
								<input class="input" type="password" value="" placeholder="请输入密码" name="password"/>
							</view>
							<view class="button">
									<button :loading="loading" formType="submit">登录</button>
							</view>
				</view>
		</form>
	</view>
</template>

<script>
	import {
	    mapMutations
	} from 'vuex';
	

	export default {
		data() {
			return {
				isHidePassword:true,
				loading: false,
				userName:"",
				password:"",
				cid: ''
			};
		},
		onLoad() {
		
		},
		methods: {
			isHidePasswordClick(){
				this.isHidePassword=!this.isHidePassword
			},

			submit(e) {
				if(e.detail.value['userName'] === "admin" && e.detail.value['password'] === "1234567"){
					 // 验证成功跳转目标页面 
					 uni.reLaunch({
						url:'/pages/index/index'
					 })
				}	
			}	
			}
	}	
</script>

<style>
	.form{
		position: absolute;
		top: 19%;
		width: 100%;
	}
	.login_title{
		width: 80%;
		margin: 2% 10%;
		font-size: 24px;
		color: #FF4500;
		text-align: center;
	}
.inputWrapper{
		width: 80%;
		height: 80upx;
		background: white;
		border-radius: 20px;
		margin: 5% 10%;
	}
	.inputWrapper .input{
		width: 100%;
		height: 100%;
		text-align: center;
		font-size: 16px;
	}
	.button button{
		width: 60%;
		margin:0 20%;
		background:#FF4500;
		border-radius:50upx;
		line-height: 100upx;
		height: 100upx;
		color: white;
		font-size: 38upx;
	}
 </style>
