<template>
	<view >
		<view v-show="arr.length > 0 ? true : false">
			<uni-collapse ref="collapse">
				<view v-for="(item,index) in arr" :key="index">
					<uni-collapse-item :title="item.projectName" :open="false" >
							<view class="content" @click="jumpPlan(item)">
								<text class="text">{{item.planName}}</text>
							</view>
						</uni-collapse-item>
				</view>
			</uni-collapse>
		</view>
		<view v-show="arr.length > 0 ? false : true" class="noPlanMes">暂时没有计划</view>
	</view>
	
</template>

<script>
	export default {
		onLoad() {
			this.getData()
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
					url:'/ntda/servicePlan/getAllServicePlan?staffId=' + getApp().globalData.staffId
				})
				console.log(res.data)
				this.arr = res.data.result
			},
			jumpPlan(value){
				uni.navigateTo ({
					url: '/pages/selfBulidTask/programDetail/programDetail?planId=' + value.planId+"&planName="+value.planName
				})
			},
			
		}
	}
</script>

<style>
    
  /* .list text {
	  padding-left: 15px;
	  font-size: 15px;
  }
  .plan{
	  margin-left: 15px;
	  font-size: 15px;
  }
	 */
	.content {
			padding: 4px;
	}

	.text {
		font-size: 14px;
		color: #666;
		line-height: 20px;
		margin-left: 30px;
	}
	.noPlanMes{
		width: 60%;
		font-size: 20px;
		text-align: center;
		margin: 30px 20%;
	}
	
</style>
