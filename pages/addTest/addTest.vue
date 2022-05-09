<template>
	<view class="box">
		<view class="content">
			<view>
				<uni-combox name="任务类型" label="任务类型" :candidates="taskType" placeholder="" v-model="task_select1"></uni-combox>
			</view>
			<view >
				<uni-combox name="项目名称" label="项目名称" :candidates="projectName" placeholder="" v-model="task_select2"></uni-combox>
			</view>
			<view class="input">
					<span>任务名称</span>
					<input name="任务名称" v-model="task_input" ></input>
			</view>
			<view>
				<uni-combox name="任务性质" label="任务性质" :candidates="taskCharacteristic" placeholder="" v-model="task_select3">测试</uni-combox>
			</view>
			<view class="personnel" @click="jumpParticipants">
				<text class="info">参与人员</text>
				<text class="personnelInfo">{{this.info}}</text>
				<span class="iconfont icon-zuojiantou-cu"></span>
			</view>
			
		</view>
		
		<view class="text">
			<text >注：补充测试项是对计划自建任务的补充，如果不存在计划自建任务，不能创建补充测试项任务!</text>
		</view>
		<button  @click="submit" >执行</button>
	</view>
	
		
</template>

<script>
	
	export default {
		onLoad() {
		},
		
		data() {
			return {
				task_input:'',
				task_select1:'',
				task_select2:'',
				task_select3:'',
				info:'非必填项',
				text:'',
				projectName:[],
				taskType:['测试任务','排查任务'],
				taskCharacteristic:['业主通知','计划内','主机警告'],
			};
		},
		
		
		methods: {
			onBackPress(e) {
					uni.switchTab({
						url: '/pages/index/index'
					});
					return true;
			},

			jumpParticipants(){
				let self = this
				uni.$on('fire', function(data) {
				    //清除监听，不清除会消耗资源
				    uni.$off('fire');
					self.info = data.data
					// console.log(self.info)
				})
				uni.navigateTo({
					url: '/pages/addTest/participants/participants'
				})
			},
			
			submit(){
				if( this.task_input == '' || this.task_select1 == ''|| this.task_select2 == '' || this.task_select3 == '' ){ 
					uni.showToast({
					    title: '信息填写不全',
						icon:'none',
					    duration: 2000
					});
				}else{
					console.log(this.task_input)
					console.log(this.task_select1)
					console.log(this.task_select2)
					console.log(this.task_select3)
				}	
			},
		}
			
			
		
	}
</script>

<style>
	
   .content{
   		margin-top:5px;
		margin-right:10px;
   		
   	}
	.input{
		display: flex;
		
	}
	input {
		margin-left: 10px;
		font-size: 14px;
		flex-grow:0;
		flex-shrink:0;
		width: 70%;
	}
	.content view {
		padding-top:10px;
		padding-left: 10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
	}
	.input span {
		margin-left:10px ;
		color:#969696;
	}
	.personnel span{
		margin-right: 0px;
	}
    .info{
		margin-left:10px ;
		color:#969696;
	}
   .personnelInfo{
	   font-size: 14px;
	   margin-left: 10px;
   }
   /* .personnel span{
	   margin-right: 0px;
   } */
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
