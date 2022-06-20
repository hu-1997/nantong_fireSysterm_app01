<template>
	<view class="big">
		<view class="nav">
			<view class="texts" :class="curr==0?'active':''" data-index="0" @tap="setCurr">
				自建
			</view>
			<view class="texts" :class="curr==1?'active':''" data-index="1" @tap="setCurr">
				接收
			</view>
			<view class="texts" :class="curr==2?'active':''" data-index="2" @tap="setCurr">
				查询
			</view>
		</view>
		<swiper style="height: 100vh;" :current="curr" @change="setCurr">
			<swiper-item>
				<scroll-view>
					<view>
						<view  @click="jumpBuildTask" class="contentlist_view">
							<view  class="icon_left">
								<image src="../../static/icons/task.png" style="width: 20px;height: 20px;"></image>
							</view>
							<view class="contentlist_view_title">按计划自建任务</view>
							<view class="icon_right">
								<image src="../../static/icons/right.png" style="width: 20px;height: 20px;"></image>
							</view>
						</view>
						<view @click="jumpAddTest" class="contentlist_view">
							<view  class="icon_left">
								<image src="../../static/icons/task2.png" style="width: 22px;height: 22px;"></image>
							</view>
							<view class="contentlist_view_title">补充测试项</view>
							<view class="icon_right">
								<image src="../../static/icons/right.png" style="width: 20px;height: 20px;"></image>
							</view>
						</view>
						<view @click="jumpMismatchTask" class="contentlist_view">
							<view  class="icon_left">
								<image src="../../static/icons/task3.png" style="width: 22px;height: 22px;"></image>
							</view>
							<view class="contentlist_view_title">处理消防设施与当前测试规范不匹配</view>
							<view class="icon_right">
								<image src="../../static/icons/right.png" style="width: 20px;height: 20px;"></image>
							</view>
						</view>
						<view @click="jumpUnsubmitTask" class="contentlist_view">
							<view  class="icon_left">
								<image src="../../static/icons/task4.png" style="width: 22px;height: 22px;"></image>
							</view>
							<view class="contentlist_view_title">管理未提交任务</view>
							<view class="icon_right">
								<image src="../../static/icons/right.png" style="width: 20px;height: 20px;"></image>
							</view>
						</view>
						<view @click="jumpMaintenanceTask" class="contentlist_view">
							<view  class="icon_left">
								<image src="../../static/icons/fixed.png" style="width: 22px;height: 22px;"></image>
							</view>
							<view class="contentlist_view_title">排查与维修任务管理</view>
							<view class="icon_right">
								<image src="../../static/icons/right.png" style="width: 20px;height: 20px;"></image>
							</view>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
			<swiper-item>
				<scroll-view>
					<view class="receive">暂未收到下发计划</view>
				</scroll-view>
			</swiper-item>	
			<swiper-item>
				<scroll-view>
					<view class="date_input">
						<view class="date">
							<uni-datetime-picker v-model="range" type="daterange" rangeSeparator="至" @change="onDateChange" />
						</view>
						<button @click="inquire">查询</button>
					</view>
					<view class="content" v-for="(item,index) in dataArr" @click="jumpSubmitPlan(item.taskId)" :key="item.taskId">
						<view>
							<text>项目名称</text>
							<text>{{item.projectName}}</text>
						</view>
						<view>
							<text>提交时间</text>
							<text>{{item.submitTime}}</text>
						</view>
						<view>
							<text>审核状态</text>
							<text>{{item.auditResult}}</text>
						</view>
					</view>
					
					
				</scroll-view>
			</swiper-item>			
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
				return {
					curr:0,
					range: [],
					startDate: '',
					endDate: '',
					dataArr:[],
				};
			
			
		},
		onShow(){
			// this.getData()
		},
		methods: {
			//获取提交任务接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/task/getDidTask?tenantId='+getApp().globalData.tenantId + '&staffId='+ getApp().globalData.staffId
				})
				console.log(res.data)
				this.dataArr = res.data.plans
				
				
			},
			setCurr(e) {
			// console.log(e.detail.current)
			let thisCurr = e.detail.current || e.currentTarget.dataset.index || 0;
			// console.log(thisCurr)
			this.curr = thisCurr;
			},
			jumpBuildTask (){
				uni.navigateTo ({
					url: '/pages/selfBulidTask/selfBulidTask',
				})
			},
			jumpAddTest(){
				uni.navigateTo ({
					url: '/pages/addTest/addTest',
				})
			},
			jumpMismatchTask(){
				uni.navigateTo ({
					url: '/pages/mismatchTask/mismatchTask',
				})
			},
			jumpUnsubmitTask(){
				uni.navigateTo ({
					url: '/pages/unsubmitTask/unsubmitTask',
				})
			},
			jumpSubmitPlan(taskId){
				uni.navigateTo ({
					url: '/pages/index/submitPlan/submitPlan?id='+ taskId
				})
			},
			jumpMaintenanceTask(){
				uni.navigateTo ({
					url: '/pages/maintenanceTask/maintenanceTask'
				})
			},
			inquire(){
				console.log(this.startDate)
				console.log(this.endDate)
				this.$myRequest({
					url:'/ntda/task/getTaskByCondition2',
					method:'POST',
					data:{
						  "startTime": this.startDate,
						  "endTime": this.endDate,
						  "staffId": getApp().globalData.staffId
					},
				}).then((res)=>{
					console.log(res.data)
					this.dataArr = res.data.plans
				})
			},
			onDateChange(e) {
			setTimeout(() => {
			this.startDate = this.range[0]
			this.endDate = this.range[1]
			})
			}, 
		}
	}
</script>

<style>
	.big{
		background-color: #FCFCFC;
	}
	.nav{
		background-color: white;
		width: 100%;
		color: #ABABAB;
		overflow: auto;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
	}
	.nav view{
		text-align: center;
		padding-left: 25upx;
		width: 30%;
		float: left;
		margin-top: 10px;
	}
	.nav .texts.active{
		color: #FF8C00;
	}
	.contentlist_view{
		display: flex;
		flex-direction: row;
		height: 30px;
		line-height: 30px;
		background-color: white;
		padding-top: 10px; 
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
	}
	.contentlist_view .icon_left {
		line-height: 30px;
		padding: 4px 0px;
		margin: 0 2% 0 4%;
	}
	
	.contentlist_view .icon_right{
		display: inline-block;
		line-height: 30px;
		padding: 3px 16px;
	}
	
	.contentlist_view .contentlist_view_title{
		width: 64%;
		margin: 0px 10% 0 0%;
	}
	
	
	.receive {
		text-align: center;
		margin:100px;
		color: #ABABAB;
	}
	.date_input{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		width: 96%;
		margin: 10px 2%;
	}
	.date_input .date {
		width: 82%;
		/* height: 32px; */
	}
	.date_input button {	
		display: block;
		width: 16%;
		line-height: 40px;
		text-align:center;
		background-color:#FF7256;
		color:white;
		font-size: 16px;
	}
	.content {
		border: 2px solid #ABABAB;
		border-radius:10px ;
		font-size: 13px;
		margin: 5px 10px 5px 10px;
		float:left;
		width:350px;
	}
	.content view{
		margin:5px;
	}
	.content text{
		padding:5px;
	}
</style>

