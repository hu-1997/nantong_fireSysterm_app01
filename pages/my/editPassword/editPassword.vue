<template>
	<view>
		<form @submit="formSubmit" @reset="formReset">
			<view class="form_input">
				<view>当前密码:</view>
				<input type="number"/>
			</view>
			<view class="form_input">
				<view>新密码:</view>
				<input type="number"/>
			</view>	
			<view class="form_input">
				<view>确认密码:</view>
				<input type="number"/>
			</view>	
			<view class="btn">
				<button form-type="submit" type="default">确认</button>
			</view>
		</form>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
.form_input{
	display: flex;
	flex-direction: row;
	width: 90%;
	padding: 2% 5%;
	border-bottom: 1px solid #DCDCDC;
}
.form_input view{
	width: 30%;
}

	.btn {
		position: fixed;
		width: 80%;
		margin: 0 10%;
		bottom: 30px;
	}
	.btn button {
	 background-color: #FF6347;
	 color: white;
	 text-align:center;
	 border-radius:10px;
	}
</style>
