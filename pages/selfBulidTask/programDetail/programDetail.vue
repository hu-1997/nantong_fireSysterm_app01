<template>
	<view class="big">
		    <!-- <view class="Allchoose" @click="allChoose" v-if="false">全选</view> -->
			<view class="nav" scroll-x="ture">
				<view class="texts" :class="curr==0?'active':''" data-index="0" @tap="setCurr">
					计划完成情况
				</view>
				<view class="texts" :class="curr==1?'active':''" data-index="1" @tap="setCurr">
					未完成维保情况
				</view>
			</view>
			<view>
			<swiper style="height: 100vh" :current="curr" @change="setCurr">
				<swiper-item>
					<scroll-view scroll-y="ture" style="height: 100vh">
						<view class="content">
									<view class="nav1">
										未完成
									</view>
									<view class="maintenanceContent1" v-for="(item, index) in arr1" :key="item.systemName + '-'+index">
										<view class="first1">
											<view>
												{{item.systemName}}
											</view>
											<view class="second" v-for="(item2, index) in item.subsystems" :key="index" >
												<view>
													{{item2.subsystemName}}
												</view>
												<view class="third" v-for="(item3, index) in item2.facilities" :key="index">
													<view>{{item3.facilitiesName}}</view>
												</view>
												
											</view>
										</view>
										
									</view>
									<view class="nav2">
										执行中
									</view>
									<view class="maintenanceContent2" v-for="(item, index) in arr2"  :key="item.systemName + '/'+index">
										<view class="first2">
											<view>
												{{item.systemName}}
											</view>
											<view class="second" v-for="(item2, index) in item.subsystems"  :key="index">
												<view>
													{{item2.subsystemName}}
												</view>
												<view class="third" v-for="(item3, index) in item2.facilities"  :key="index">
													<view>{{item3.facilitiesName}}</view>
												</view>
												
											</view>
										</view>
										
									</view>
									<view class="nav3">
										已完成
									</view>
									<view class="maintenanceContent3" v-for="(item, index) in arr3"  :key="item.systemName + '_'+index">
										<view class="first3">
											<view>
												{{item.systemName}}
											</view>
											<view class="second" v-for="(item2, index) in item.subsystems"  :key="index">
												<view>
													{{item2.subsystemName}}
												</view>
												<view class="third" v-for="(item3, index) in item2.facilities"  :key="index">
													<view>{{item3.facilitiesName}}</view>
												</view>
												
											</view>
										</view>
										
									</view>
								</view>
					</scroll-view>
				</swiper-item>
				<swiper-item>
					<scroll-view scroll-y="ture" style="height: 100vh" >
						<view class="content">
									<view class="maintenanceContent1" v-for="(item, index) in arr1"  :key="index">
										<view class="first1">
											<view>
												{{item.systemName}}
											</view>
											<view class="second" v-for="(item2, index) in item.subsystems"  :key="index">
												<view>
													{{item2.subsystemName}}
												</view>
												<view class="third">
													<label v-for="(item3, index) in item2.facilities" :key="index">
														<checkbox-group name="" @change="checkboxChange($event, item3.subplanId)" >
															<view><checkbox  :checked="checkAll"  :value="item3.subplanId"/> 
															<text>{{item3.facilitiesName}}</text></view>
														</checkbox-group>
													</label>
												</view>
												
											</view>
										</view>
										
									</view>
						</view>
						
					</scroll-view>
					<!-- <view class="btn" scroll-y="false">
						<button  type="default" @click="submit">自建测试任务</button>
					</view>	 -->
				</swiper-item>			
			</swiper>
			</view>
			<view>
				<button @click="submit"  :class="curr == 1?'button_show':'button_hide'">自建测试任务</button>
			</view>
		</view>
</template>

<script>
	export default {
		//全选按钮
		onNavigationBarButtonTap(e){
			this.checkAll = !this.checkAll
		},
		onLoad(options) {
			this.planId = options.planId
			this.getData()
			if(options.planId){
				uni.setNavigationBarTitle({
					title: String(options.planName)
				})
			}
		},
		
		data() {
			return {
				curr:0,
				ab:[],
				w:'',
				flag:true,
				checkAll:false,
				swiperHeight: 0,
				arr1:[],
				arr2:[],
				arr3:[],
				planId:'',
				checkboxArr:[],
				namelist:[],
			}	
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/subServicePlan/getAllSubPlan2?planId=' + this.planId +'&staffId=' + getApp().globalData.staffId
				})
				console.log(res.data)
				this.arr1 = res.data.uncompleted
				this.arr2 = res.data.doing
				this.arr3 = res.data.completed
				// console.log(this.planId)
			},
			//滑动页面参数
			setCurr(e) {
				let thisCurr = e.detail.current || e.currentTarget.dataset.index || 0;
				this.curr = thisCurr;
			},
			//复选框的值改变
			checkboxChange(e, id) {
				// console.log(e.detail.value, id)
				let me = this
				if(e.detail.value.length > 0){
					me.ab.push(e.detail.value[0])
				} else{
					var a = me.ab.indexOf(id)
					me.ab.splice(a,1)
				}
				this.w = this.ab.join(",")	
				// console.log(this.w)
			},
			handleData(){
				var newArrayId = []
				this.arr1.forEach(item => {
					newArrayId.push(item.subsystems[0].facilities[0]['subplanId'])
				})
				return newArrayId
			},
			submit(e){
				if (this.w != ''){
					//新建维保内容并发送数据
					// console.log(this.planId)
					 this.$myRequest({
						url:'/ntda/task/updateSubPlans',
						method:'POST',
						data:{
							 "formId": this.w,
							 "completeStatus": "执行中",
							  "planId": this.planId,
							 "tenantId": getApp().globalData.tenantId,
							 	"staffId": getApp().globalData.staffId
						},
					}).then((res) =>{
						var taskid = res.data
						uni.navigateTo ({
							url: '/pages/selfBulidTask/programDetail/newTask/newTask?id='+ taskid
						})
					})
				}
				if( this.checkAll === true ) {
					var allCheckedPlanId = this.handleData()
					// console.log(allCheckedPlanId)
					//全选维保内容发送数据
					this.$myRequest({
						url:'/ntda/task/updateSubPlans',
						method:'POST',
						data:{
							"formId": allCheckedPlanId.join(","),
							"completeStatus": "执行中",
							 "planId": this.planId,
							 "tenantId": getApp().globalData.tenantId,
							 "staffId": getApp().globalData.staffId
						},
					}).then((res) =>{
						var taskid = res.data
						uni.navigateTo ({
							url: '/pages/selfBulidTask/programDetail/newTask/newTask?id='+ taskid
							})
					})
				} else{
					uni.showToast({
							title: '请选择',
						icon:'none',
							duration: 2000
					})	
				}
			}
		}
	}
</script>

<style>
	  .Allchoose{
			margin-top:5px;
			float: right;
			margin-right: 10px;
			color: #FF7256;
			
		}
		.big{
			background-color: #FCFCFC;
			
		}
		.nav{
			overflow: auto;
			background-color: white;
			width: 100%;
		}
		.nav view{
			text-align: center;
			color: #ABABAB;
			/* margin-top: 10px; */
			/* padding-left: 25upx; */
			width: 50%;
			margin: 5px 0;
			float: left;
		}
		.nav .texts.active{
			color:  #FF8C00;
		}
		.nav1{
				   margin: 10px;
				   color: red;
				   font-size:14px;
		}
		.content {
			overflow-y: scroll; 
			margin-top: 6px;
		}
		
		.maintenanceContent1{
				   border: 1px solid #CDC9C9;
				   margin:10px;
				   border-radius: 5px;
				   background-color: white;
					 .third{
						 margin-bottom: 10px;
					 }
		}
		.first1{
				   border-left: 3px solid #FF4500;
				   margin-left:10px;
		}
		.maintenanceContent1 view{
				   margin:10px;
				   font-size: 13px;
				   
		}
		.nav2{
				   margin: 10px;
				   color: #ADD8E6;
				   font-size:14px;
		}
		
		.maintenanceContent2{
				   border: 1px solid #CDC9C9;
				   margin:10px;
				   border-radius: 5px;
				   background-color: white;
		}
		.first2{
				   border-left: 3px solid #ADD8E6;
				   margin-left:10px;
		}
		.maintenanceContent2 view{
				   margin:10px;
				   font-size: 13px;
		}
		.nav3{
				   margin: 10px;
				   color: #4EEE94;
					   font-size:14px;
		}
		
		.maintenanceContent3{
				   border: 1px solid #CDC9C9;
				   margin:10px;
				   border-radius: 5px;
				   background-color: white;
		}
		.first3{
				   border-left: 3px solid #4EEE94;
				   margin-left:10px;
		}
		.maintenanceContent3 view{
				   margin:10px;
				   font-size: 13px;
		}
		/* .item2 {
			flex: 1;
			position: relative;
		} */
		/* .btn{
			position:absolute;
			bottom:0px;
			left:0px;
			width:100%;
			height:100%;
			overflow:auto;
		} */
	/* 	button {
			   background-color: #FF6347;
			   color: white;
			   text-align:center;
			   height: 40px;
			   width: 320px;
			   border-radius:10px;
			   position: absolute;
			   right: 5px;
			   bottom: 105px;
			   left: 5px;	   
		} */
		.button_show{
			background-color: #FF6347;
			color: white;
			/* text-align:center; */
			/* height: 30px; */
			width: 80%;
			border-radius:10px;
			position: fixed;
			/* right: 5px;
			bottom: 105px;
			left */: 5px;	
			margin: 2% 10%;
			bottom: 20px;
		}
		.button_hide{
			height: 0;
			width: 0;
		}

</style>
