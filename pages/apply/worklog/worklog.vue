<template>
	<view class="nav">
		<view class="date">
			<uni-datetime-picker v-model="range" type="daterange" rangeSeparator="至" @change="onDateChange" />
		</view>
		<view class="btn">
			<button>查询</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
				return {
					range: [],
					startDate: '',
					endDate: ''
					
				};
			
		},
		methods: {
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
	.date {
		float: left;
		margin-left: 5px;
		margin-top: 10px;
		width: 280px;
	}
	button {
		margin-top: 15px;
		margin-right: 15px;
		width:65px;
		height:30px;
		line-height:28px;
		text-align:center;
		float: right;
		background-color:#FF7256;
		color:white;
		font-size: 15px;
	}

</style>
