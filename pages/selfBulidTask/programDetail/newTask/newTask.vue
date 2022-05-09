<template>
	<view class="Box" >
		<view class="content" >
			<view>
				任务类型
				<text>{{arr.taskType}}</text>
			</view>
			<view>
				项目名称
				<text>{{arr.projectName}}</text>
			</view>
			<view class="input">
				<span class="taskname">任务名称</span>
				<input :value="arr.taskName" type="text"  @input="changeInput"   placeholder="">
				<!-- <text>{{arr.taskName}}</text> -->
			</view>
			<view>
				任务性质
				<text>{{arr.planNature}}</text>
			</view>
			<view @click="jumpParticipants">
				参与人员
				<span class="info">{{this.infoName}}</span>
				<span class="iconfont icon-zuojiantou-cu"></span>
			</view>
			<!-- <button @click="jumpModify(arr.taskId)" type="default" >执行</button> -->
		</view>
		<button @click="jumpModify(arr.taskId)" type="default" >执行</button>
	</view>
</template>

<script>
	export default {
		onLoad(options){
			this.taskId = options.id
			this.getData();
			// console.log(this.inputValue)
		},
		data() {
			return {
				arr:{},
				taskId:'',
				info:'非必填项',
				inputValue:'',
				infoName:''
			}
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/task/getLatestTask?taskId=' + this.taskId
				})
				// console.log(9, res.data)
				this.arr = res.data
			},
			changeInput(e){
				this.inputValue = e.target.value
				// console.log(e.target.value)
			},
			jumpParticipants(){
				let self = this
				uni.$on('fire', function(data) {
					//清除监听，不清除会消耗资源
					uni.$off('fire');
					// console.log(data.data)
					const newArr = []
					const newIdArr = []
					const numberArr = data.data.split(",")
					numberArr.forEach(item => {
						newArr.push(item.split("/")[0])
						newIdArr.push(item.split("/")[1])
					})
					self.info = newIdArr.join(",")
					self.infoName = newArr.join(",")
					// console.log(self.info)
				})
				uni.navigateTo({
					url: '/pages/selfBulidTask/programDetail/newTask/participants/participants'
				})
			},
			jumpModify(taskId){
				this.$myRequest({
					url:'/ntda/taskMembers/addTaskMembers',
					method:'POST',
					data:{
						 "taskId": this.taskId,
						 "tenantId":getApp().globalData.tenantId,
						 "staffIds": this.info
					},
				})
				this.$myRequest({
					url:'/ntda/task/updateTaskName',
					method:'POST',
					data:{
						 "taskId": this.taskId,
						 "taskName":this.inputValue,
					},
				}).then((res) =>{
					uni.navigateTo({
						url: '/pages/unsubmitTask/modify/modify?id=' + taskId
					})
				})
				
			},
			onBackPress(e) {
					uni.switchTab({
						url: '/pages/index/index'
					});
					return true;
			},
		}
	}
</script>

<style>
	.Box {
		/* margin:10px; */
	}
	.content{
		padding: 10px;
	}
	.content view{
		margin-top:10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
	}
	.input{
		display: flex;
		
	}
	.taskname {
		font-size: 16px;
	}
	input {
		margin-left:15px;
		font-size: 13px;
		flex-grow:0;
		flex-shrink:0;
		width: 70%;
	}
	text {
		margin-left:15px;
		font-size: 14px;
		
	}
	.info {
		font-size: 13px;
		margin-left:15px;
		color: #8F8F8F;
	}
	.Box button {
		 /* float: bottom; */
		 /* padding-bottom: 5px; */
		 background-color: #FF6347;
		 color: white;
		 text-align:center;
	 /* height: 40px;
		 width: 320px; */
		 width: 80%;
		 border-radius:10px;
		 position: fixed;
		 margin: 0 10%;
		 /* right: 5px; */
		 bottom: 50px;
		 /* left: 5px; */
	}

</style>
