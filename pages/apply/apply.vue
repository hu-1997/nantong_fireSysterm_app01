<template>
	<view>
		<view class="contentlist">
			<view @click="jumpWorklog">
				<span class="iconfont icon-zixunguanli"></span>
				<span>工作日志</span>
				<span class="iconfont icon-zuojiantou-cu"></span>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			jumpWorklog (){
				uni.navigateTo ({
					url: '/pages/apply/worklog/worklog',
				})
			},
		}
	}
</script>

<style>
	
	.contentlist view{
		padding-top: 10px; 
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
		
	}
	span{
		font-size: 14px;
		padding-left: 18px;
	}
  
</style>
