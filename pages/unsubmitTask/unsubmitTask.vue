<template>
	<view class="big">
		<view class="box" v-for="(item, index) in arr" :key="index">
				<view >
					项目名称   <span>{{item.projectName}}</span>	
				</view>
				<view class="content">
					任务名称   <span>{{item.taskName}}</span>
				</view>
				<view class="content">
					创建时间   <span>{{item.createdTime}}</span>
				</view>
				<view class="icons">
					<view @click="Delete(item.taskId)" class="icons_view">
						<view class="icon_left">
							<image src="../../static/icons/delete.png" style="width: 18px;height: 18px;"></image>
						</view>
						<view class="delete">删除</view>
					</view>
					<view @click="jumpModify(item.taskId)" class="icons_view">
						<view class="icon_left">
							<image src="../../static/icons/edit.png" style="width: 18px;height: 18px;"></image>
						</view>
						<view class="delete">修改</view>
					</view>
				</view>		
		</view>	
	</view>
</template>

<script>
	export default {
		onShow() {
			this.getData();
		},
		data() {
			return {
				arr:[],
			}
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/task/getDoingTask?tenantId='+ getApp().globalData.tenantId + '&staffId=' + getApp().globalData.staffId
				})
				// console.log(res.data)
				this.arr = res.data.plans
			},
			jumpModify(taskId){
				uni.navigateTo({
					url: '/pages/unsubmitTask/modify/modify?id=' + taskId 
				})
			},
			Delete(taskId){
				uni.showModal({
					cancelText: "取消", // 取消按钮的文字  
					confirmText: "确认", // 确认按钮文字 
					title: '删除提示',
					content: '是否删除该项记录?',
					confirmColor:'#3B8BFF',
					cancelColor:'#222222',
					success: res => {
						let self = this
						if (res.confirm) {
							this.$myRequest({
								url:'/ntda/task/deleteTask',
								method:'POST',
								data:{
									 "taskId": taskId
								}
							}).then(()=>{
								self.getData()
							}) 
						} else if (res.cancel) {
							// 取消
							console.log('cancel')
						}
					}
				});

			}
			
		}
	}
</script>

<style>
	.big{
		background-color: #FCFCFC;
	}
	.box {
		background-color: white;
		padding: 5px;
		border: 1px solid #FF8C69;
		border-radius:10px ;
		margin:10px ;
		height: 110px;
	}
	.box view{
		padding-top:5px;
		padding-left: 5px;
	}
	span{
		padding-left: 10px;
		font-size: 14px;
		color:#515151;
	}
	.box .icons{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		width: 44%;
		padding: 0 4% 6px 52%;
	}
	.box .icons_view {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		width: 40%;
		height: 30px;
		padding: 4px 0;
	}
	.icons_view .icon_left{
		line-height: 30px;
		padding: 5px 0;
	}
	
	.icons_view .delete{
		font-size: 18px;
		color:#FF4500;
		line-height: 30px;
		padding: 2px 0;
	}

</style>
