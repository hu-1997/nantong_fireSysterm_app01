<template>
	<view class="box">
		<view class="content">
			<view class="default">
				<text>任务类型</text>
				<span >测试任务</span>
			</view>
			<view>
				<uni-combox label="项目名称" :candidates="projectName" placeholder="" v-model="task_select">测试</uni-combox>
			</view>
			<view class="input">
					<text>任务名称</text>
					<input v-model="task_input"></input>
			</view>
		</view>
		
		<view class="text">
			<text >注：补充测试项是对计划自建任务的补充，如果不存在计划自建任务，不能创建补充测试项任务!</text>
		</view>
		
		<button class="zhixing" type="default" @click="submit">执行</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				task_input:'',
				task_select:'',
				projectName:[],
				
			}
		},
		methods: {
			submit(){
				if( this.task_input == '' || this.task_select == '' ) {//内容按自己实际的内容填写
					uni.showToast({
					    title: '信息填写不全',
						icon:'none',
					    duration: 2000
					});//使用uview的组件
					return;
				}
			}
		}
	}
</script>

<style>
	.content{
			margin-top:5px;
			margin-right:10px;
			
		}
	.content view{
		padding-top:10px;
		padding-left: 10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
	}
	.default span{
		margin: 10px;
		font-size: 14px;
		
	}
	.content text {
		margin-left: 10px;
		color:#969696;
	}
	.content input {
		margin-left: 10px;
		font-size: 14px;
	}
  .input{
  	display: flex;
  }
  .text {
  	   font-size: 14px;
  	   color: #EE2C2C;
  	   margin: 15px;
  }
  button {
  	   float: bottom;
  	   padding-bottom: 5px;
  	   background-color: #FF6347;
  	   color: white;
  	   text-align:center;
  	   height: 40px;
  	   width: 320px;
  	   border-radius:15px;
  	   position: fixed;
  	   right: 5px;
  	   bottom: 15px;
  	   left: 5px;  
  }
</style>
