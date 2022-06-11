<template>
	<view class="Box" >
		<view class="content" >
			<view class="border_bottom">
				<view>任务类型</view>
				<view>{{taskDetailInfo.taskType}}</view>
			</view>
			<view class="border_bottom">
				<view>项目名称</view>
				<view>{{taskDetailInfo.projectName}}</view>
			</view>
			<view class="border_bottom">
				<view>任务名称</view>
				<view>{{taskDetailInfo.taskName}}</view>
			</view>
			<view class="border_bottom">
				<view>任务性质</view>
				<view>{{taskDetailInfo.taskNature}}</view>
			</view>
			<view>
				<view class="border_bottom">
					<view>参与人员</view> 
					<view>{{this.membersName}}</view>
				</view>
				<view class="number_checkbox">
					<checkbox-group @change="checkboxChange">
									<label class="uni-list-cell " v-for="item in membetItems" :key="item.staffId">
										<view class="checkbox_list">
											<checkbox :value="item.account + '-'+item.name+'/'+item.staffId" :checked="item.checked" />
											<view>{{"ID:"+item.account + '-'+item.name}}</view>
										</view>
									</label>
								</checkbox-group>
				</view>
			</view>
		</view>
		<button @click="addMember" type="default" >执行</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				taskDetailInfo:{taskId:'001',taskType:'排查任务',projectName:'上理经管楼检测项目',taskName:'P220400011_2022年04月年度检测月计划_2_1',
				planNature:'计划内'},
				taskId:'',
				info:'非必填项',
				inputValue:'',
				membersName:'',
				hadMember:[],
				membetItems: [],
				staffIdInfo:null,
				flag:false
			}
		},
		onLoad(options){
			console.log(options)
			this.taskId = options.taskId
			this.getTasksData(options.taskId);
			this.getTasksMemberData(options.taskId)
			// console.log(this.inputValue)
		},
		methods: {
			//获取机构下成员信息
			getMemberData(){
				this.$myRequest({
					url: '/ntda/staff/getAllHasAccountStaff/',
					method: 'post',
					data:{
						"tenantId":getApp().globalData.tenantId
					}
				}).then(res => {
						this.membetItems = this.handleData(res.data.result)
				})		
			},
			
			//数据处理
			handleData (values) {
				var newArr = values
				newArr.forEach(item => {
					for(var i = 0;i < this.hadMember.length;i++){
						if(this.hadMember[i].staffId == item.staffId){
							item['checked'] = true
							break;
						} else{
							item['checked'] = false
						}
					}
				})
				return newArr;
			},
			
			//获取任务相关数据
			async getTasksData(params){
				const res = await this.$myRequest({
					url:'/ntda/task/getCheckOrRepariTaskDetail',
					method:'post',
					data:{taskId:params}
				})
				// console.log(9, res.data)
				this.taskDetailInfo = res.data
			},
			
			//获取当前任务下已经添加的成员
			getTasksMemberData(params){
				this.$myRequest({
					url:'/ntda/taskMembers/getTaskMembersByTaskId',
					method:'post',
					data:{taskId:params}
				}).then(res => {
					// console.log(9, res.data)
					this.hadMember = res.data
					this.getMemberData()
				})
			},
			
			//获取多选框的值
			checkboxChange(e){
				// console.log(e.detail.value)
				var values = e.detail.value
				var arr1 = []
				var arr2 = []
				values.forEach(item => {
					arr1.push(item.split("/")[0])
					arr2.push(item.split("/")[1])
				})
				this.membersName = arr1.join("-")
				this.staffIdInfo = arr2.join(",")
				this.flag = true
			},
			
			//添加成员后跳转
			addMember(){
				if(this.flag == false){
					uni.navigateTo({
						url: '/pages/maintenanceTask/tasksInfo/tasksInfo?taskId='+this.taskId
					})
				}else if(this.flag = true) {
					const me = this
					me.$myRequest({
						url:'/ntda/taskMembers/addTaskMembers',
						method:'POST',
						data:{
							 "taskId": me.taskId,
							 "tenantId":getApp().globalData.tenantId,
							 "staffIds": me.staffIdInfo
						},
					}).then((res) =>{
						// console.log(res)
						uni.showToast({
								title: '成员添加成功',
								icon:'success',
								duration: 2000,
								success: () => {
									setTimeout(function(){
										// uni.navigateTo({
										// 		url: '/pages/maintenanceTask/tasksInfo/tasksInfo?taskId='+this.taskId
										// })
										console.log(me.taskId)
									},2000)
								}
						})
					})
				}
			},
			onBackPress(e) {
					uni.navigateTo({
						url: '/pages/maintenanceTask/maintenanceTask'
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
.content .border_bottom{
		margin-top:10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.border_bottom view:nth-child(2){
		width: 80%;
		font-size: 14px;
	}
	.input{
		display: flex;
		margin-top:10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
		
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


	.content .number_checkbox {
		width: 92%;
		height: auto;
		padding: 10px 4%;
		border: 1px solid  #FF6347;
		border-radius: 6px;
	}
	.uni-list-cell {
		display: flex;
		flex-direction: row;
	}
	.checkbox_list{
		width: 90%;
		display: flex;
		flex-direction: row;
		border-bottom: 0;
	}
	.checkbox_list checkbox{
		margin: 9px 4px;
	}
	.checkbox_list view{
		line-height: 43px;
	}
	

</style>
