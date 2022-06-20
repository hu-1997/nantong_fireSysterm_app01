<template>
	<view class="Box">
		<view class="nav">
				<view class="search">
					<input type="text" placeholder="请输入人员名称" @input="getInputValue">
				</view>	
				<button type="default" @click="getSearchData">查询</button>	
		</view>
		<view class="option">
			<checkbox-group v-for="(item,index) in dataArr" @change="checkboxChange" :key="index">
				<label>
					<!-- <view>{{item.name}} <checkbox :value="item.name" /></view> -->
						<view>{{'ID:'+item.account + '-'+item.name}} <checkbox :value="item.name+'/'+item.staffId" /></view>
				</label>
			</checkbox-group>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				w:[],
				ab:'',
				dataArr:[],
				searchName:''
			}
		},
		onLoad(options){
			this.getData();
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url: '/ntda/staff/getAllHasAccountStaff/',
					method: 'post',
					data:{
						"tenantId":getApp().globalData.tenantId
					}
				})
				// console.log(res.data)
				this.dataArr = res.data.result
			},
			getInputValue(e){
				this.searchName = e.detail.value
			},
			//搜索
			getSearchData(){
				this.$myRequest({
					url: '/ntda/staff/getStaffByCondition/',
					method: 'post',
					data:{
						"ifHasTenant": 'true',
						"name": this.searchName,
						"tenantName":getApp().globalData['tenantName']
					}
				}).then(res => {
					// console.log(res.data)
					this.dataArr = res.data.result
				})
			},
			checkboxChange(e) {
				this.w.push(e.detail.value)
				this.ab=this.w.join(",")	
			},
			onNavigationBarButtonTap(e){
				uni.$emit('fire', {
				    data: this.ab
				})
				uni.navigateBack ({
					url: '/pages/selfBulidTask/programDetail/newTask/newTask' 
				})
			}
		},
	
	}
</script>

<style>
	.search input{
		width: 270px;
		border: 0.5px solid #EE5C42;
		padding:5px;
		font-size: 13px;
		float: left;
		border-radius: 4px;
	}
	.nav {
		margin: 10px ;
	}
  .option view{
	  padding-top: 10px ;
	  padding-left: 15px ;
	  font-size: 14px;
  }
  .nav button {
	  width: 58px;
	  height: 30px;
	  font-size: 14px;
	  text-align: center;
	  vertical-align: middle;
	  line-height: 27.5px;
	  margin-right:5px; 
		color: white;
		background-color:#EE5C42 ;
  }
  .option checkbox {
	  float: right;
	  padding-right: 15px;
  }
</style>
