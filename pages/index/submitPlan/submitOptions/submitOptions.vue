<template>
	<view class="box">
		<view  >
			<view class="nav">
				<view>设施名称</view>
				<text >{{this.w}}</text>
			</view>
			
		</view>
		<view >
			<view v-for="(item) in arr" class="content" @click="jumpSubmitOptionDetail(item.maintenanceId,item.taskId)">
				<view>
					{{item.maintenanceName}}
				</view>
				<view class="describe">
					{{item.maintenanceDescription}}
				</view>
				<view class="" >
					已记录
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				w:'',
				facilitiesId:'',
				taskId:'',
				arr:[],
			}
		},
		onLoad(options) {
			this.taskId = options.taskId
			this.subplanId = options.id
			
		},
		onShow(){
			this.getData()
		},
		methods: {
			//获取接口数据	
			async getData(){
					const res = await this.$myRequest({
					url:'/ntda/task/getFacilitiesDetail?id=' + this.subplanId + '&taskId=' + this.taskId
				})
				this.w = res.data.facilitiesName
				this.arr = res.data.facilitiesDetail
				
				
			},
			
			jumpSubmitOptionDetail(maintenanceId,taskId){
				uni.navigateTo({
					url: '/pages/index/submitPlan/submitOptions/submitOptionDetail/submitOptionDetail?id=' + maintenanceId + '&taskId=' + taskId
				})
			}
		}
	}
</script>

<style>
	.box {
		padding: 8px;
	}
	.nav {
		display: flex;
		margin: 5px ;
		border-bottom: 1px solid #CDC9C9;
		padding-bottom: 5px;
	}
    .nav text{
		padding-left: 13px;
		font-size: 14px;
	}
	.content view{
		padding:5px 10px 5px 10px ;
		
	}
	.content {
		margin: 5px;
		margin-top:10px;
		border: 1px solid #FF4500;
		border-radius:5px;
	}
	.describe{
		color: #5B5B5B;
		font-size: 14px;
	}
	.weijilu {
		color:#EE7942;
	}
	
</style>
