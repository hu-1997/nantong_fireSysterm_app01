<template>
	<view class="my_box">
		<view class="my_top">
			<uni-file-picker class="image_picker" limit="1" :del-icon="false" disable-preview :imageStyles="imageStyles"
				:value="filePath" file-mediatype="image"></uni-file-picker>
				<view class="my_name">{{username}}</view>
				<view class="my_enterPrise">{{enterPrise}}</view>
		</view>
		<view class="my_center" @click="goToEditPass">
			<view>修改密码</view>
			<uni-icons type="forward" size="28"></uni-icons>
		</view>
		<view class="btn">
			<button @click="loginOut" type="default">退出登入</button>
		</view>
	</view>
</template>

<script>
	export default{
		
		data(){
			return{
				imageStyles: {
					width:100,
					height: 100,
					border: {
						radius: '50%'
					}
				},
				filePath:{
					url:"https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2F2020-01-10%2F5e1821464f2a2.jpg&refer=http%3A%2F%2Fpic1.win4000.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1657593935&t=9c477427492ee0b743a2915e17cf4340"
				},
				username:'张三',
				enterPrise:'南通市大安消防技术服务有限公司'
			}
		},
		methods:{
			loginOut(){
				uni.navigateTo({
					url:'/pages/login/login'
				})
			},
			goToEditPass(){
				uni.navigateTo({
					url:'/pages/my/editPassword/editPassword'
				})
			}
		}
	}
</script>

<style>
	.my_box{
		height: 100%;
		width: 100%;
		margin: 0;
		padding: 0;
	}
	.my_top{
		width: 100%;
		height: 50%;
		background-color: #EE5C42;
	}
	.my_top .image_picker{
		/* width: 60%; */
		padding: 20% 38% 5% 38%;
	}
	.my_name{
		width: 10%;
		text-align: center;
		font-size: 16px;
		color: white;
		padding: 2% 45%;
	}
	.my_enterPrise{
		width: 80%;
		text-align: center;
		font-size: 16px;
		color: white;
		padding: 2% 10% 10% 10%;
	}
	.my_center{
		display: flex;
		flex-direction: row;
		width:90%;
		padding: 2% 5%;
		border-bottom: 1px solid #DCDCDC;
	}
	.my_center view{
		width: 90%;
		font-size: 18px;
		text-align: left;
		line-height: 28px;
	}
	.btn {
		position: fixed;
		width: 80%;
		margin: 10%;
		bottom: 40px;
	}
	.btn button {
	 background-color: #FF6347;
	 color: white;
	 text-align:center;
	 border-radius:10px;
	}
</style>