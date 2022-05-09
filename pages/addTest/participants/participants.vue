<template>
	<view class="Box">
		<view class="nav">
				<text class="search">
					<input type="text" placeholder="请输入人员名称">
				</text>	
				<button type="default">查询</button>
				
				
					
		</view>
		<view class="option">
			<checkbox-group v-for="(item,index) in dataArr" name=""@change="checkboxChange">
				<label>
					<view>{{item.name}} <checkbox :value="item.name" /></view>
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
				dataArr:[
				]
			}
		},
		onLoad(options){
			this.getData();
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/staff/getAllStaffByTenant?tenantId=' + getApp().globalData.tenantId
				})
				
				this.dataArr = res.data.result
			},
			checkboxChange(e) {
				this.w.push(e.detail.value)
				this.ab=this.w.join(",")
				// console.log(this.w)
				// console.log(this.ab)
				
			},
			onNavigationBarButtonTap(e){
				uni.$emit('fire', {
				    data: this.ab
				})
				uni.navigateBack ({
					url: '/pages/addTest/addTest' 
				})
			}
		},
	
	}
</script>

<style>
	 .search input{
		width: 270px;
		border: 0.5px solid #FF8C69;
		padding:5px;
		font-size: 13px;
		float: left;
		
	}
	.nav {
		margin: 10px ;
	}
	
   
  .option view{
	  padding-top: 10px ;
	  padding-left: 15px ;
	  font-size: 14px;
  }
  button {
	  width: 58px;
	  height: 30px;
	  font-size: 14px;
	  text-align: center;
	  vertical-align: middle;
	  line-height: 27.5px;
	  margin-right:5px; 
  }
  .option checkbox {
	  float: right;
	  padding-right: 15px;
  }
</style>
