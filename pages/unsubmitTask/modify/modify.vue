<template>
	<view class="box">
		<view class="nav">
			<view class="imgs">任务名称   
			  <span>{{this.taskName}}</span>
			</view>
			<view class="img">
				<view class="title">
					提交人自拍
				</view>
				<view class="Box" >
				    <view class="cu-form-group" >
				        <view class="grid col-4 grid-square flex-sub">
				            <view class="bg-img" v-for="(item,index) in submitImgList" :key="index" @tap="ViewImage1" :data-url="submitImgList[index]">
				             <image :src="submitImgList[index]" value="" mode="aspectFill"></image>
				                <view class="cu-tag bg-red" @tap.stop="DelImg1" :data-index="index">
				                    <text class='cuIcon-close'></text>
				                </view>
				            </view>
				            <view class="solids" @tap="ChooseImage1" v-if="submitImgList.length<9">
				                <text class='cuIcon-cameraadd'></text>
				            </view>
				        </view>
				    </view>
				    
				</view>
				<view class="title">
					现场签到拍照
				</view>
				<view class="Box" >
				    <view class="cu-form-group" >
				        <view class="grid col-4 grid-square flex-sub">
				            <view class="bg-img" v-for="(item,index) in signImgList" :key="index" @tap="ViewImage2" :data-url="signImgList[index]">
				             <image :src="signImgList[index]" value="" mode="aspectFill"></image>
				                <view class="cu-tag bg-red" @tap.stop="DelImg2" :data-index="index">
				                    <text class='cuIcon-close'></text>
				                </view>
				            </view>
				            <view class="solids" @tap="ChooseImage2" v-if="signImgList.length<9">
				                <text class='cuIcon-cameraadd'></text>
				            </view>
				        </view>
				    </view>
				    
				</view>
			</view>
			
		</view>
		<view class="content" v-for="(item, index) in dataArr" :key="index">
				<view class="item1">
					<view >
						系统名称   <span>{{item.systemName}}</span>
					</view>
					<view  v-for="(item2,index) in item.subsystems" :key="index">
						<view class="item2">
							系统类型   <span>{{item2.subsystemName}}</span>
						</view>
						<view  v-for="(item3,index) in item2.facilities" :key="index" >
							<view class="item3">
								<view>设施名称   <span>{{item3.facilitiesName}}</span></view>
								<view class="number_proportion"> 
									<view>数量:<span>{{item3.count}} </span></view>  
									<view>抽检比例:<span>{{item3.checkProportion}}</span></view>
								</view>
								<view>
										<view class="icon">
											<text class="record2" v-if="item3.record2">未记录</text>
											<text class="record1" v-if="item3.record1">已记录</text>
											<view @click="modify(item3.subplanId)">
												<uni-icons type="trash" size="18" color="#FF4500"></uni-icons>
												<text>删除</text>
											</view>
											<view @click="jumpOptions(item3.subplanId,item3.taskId,item3)">
												<uni-icons type="compose" size="18" color="#FF4500"></uni-icons>
												<text>明细</text>
											</view>
										</view>
								</view>
							</view>
							
						</view>
					</view>
				</view>
				
				
				
		</view>
		<view class="btn">
			<button @click="submitTask" type="default">提交</button>
		</view>
		
	</view>
</template>

<script>
	var self; 
	export default {
		created() {
			self = this
		},
		onLoad(options) {
			this.taskId = options.id
			
			
		},
		onShow(){
			this.getData()
		},
		back() {
				
			},
		data() {
			return {
				dataArr:[],
				w:'',
				taskName:'',
				taskId:'',
				ifSubmit:'',
				submitImgList: [],
				signImgList:[]
			}
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/task/getDoingSubPlan?taskId=' + this.taskId
				})
				console.log(res.data)
				this.dataArr = res.data.doing
				this.taskName = res.data.taskName
				this.ifSubmit = res.data.flag			
			},
			submitTask(){
				if (this.ifSubmit == false) {
					uni.showToast({
					  title: '任务尚未完成，请检查是否有遗漏',
						icon:'none',
					  duration: 2000
					})
				}else{
					this.$myRequest({
						url:'/ntda/task/submitTask',
						method:'POST',
						data:{
							 "taskId": this.taskId,
							 "staffId": getApp().globalData.staffId,
							 "submitImgList": this.submitImgList,
							 "signImgList": this.signImgList,
						}
					}).then(()=>{
						uni.showToast({
								title: '任务提交成功',
								icon:'none',
								duration: 3000,
								// success() {
								// 	uni.switchTab({
								// 		url: '/pages/index/index'
								// 	});
								// }
						})
						setTimeout(() => {
								uni.hideToast();
								//关闭提示后跳转
								uni.switchTab({
									url: '/pages/index/index'
								});
							}, 1500)
					})
				}
			},
			jumpOptions(subplanId,taskId, value){
				const number = Math.ceil((value.count * (value.checkProportion || '').split("%")[0]) / 100)
				uni.navigateTo({
					url: '/pages/unsubmitTask/modify/options/options?id=' + subplanId + '&taskId='+ taskId+'&number='+number
				})
			},
			onBackPress(e) {
				if (e.from == "backbutton") {
							uni.showModal({
								title: '提示',
								content: '您当前的任务未提交，离开后请前往管理未提交任务查询该任务信息',
								success: (res) => {
									if (res.confirm) {
										uni.switchTab({
											url: '/pages/index/index'
										});
									}
								}
							});
							return true; //阻止默认返回行为
						}
			},
			ChooseImage1() {
				const _this = this
			    uni.chooseImage({
			        count: 9, //默认9
			        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			        sourceType: ['album','camera'], //从相册选择
			        success: (res) => { 
								for(let i=0;i<res.tempFilePaths.length;i++){
									const tempFilePaths = res.tempFilePaths[i]
									const uploadTask = uni.uploadFile({
										url : 'http://110.42.216.179:8000/ntda/files/uploadImages',
										filePath: tempFilePaths,
										name: 'files',
										formData: {
											'fileId': getApp().globalData.staffId
										},
										success: function (uploadFileRes) {
										 // console.log(uploadFileRes);
										 // _this.imgList = [..._this.imgList, uploadFileRes.data]
										 _this.submitImgList.push(uploadFileRes.data)
										 
										}
									});
								}
							   // console.log(_this.imgList)  
			        }
			    });
			},
			ViewImage1(e) {
			    uni.previewImage({
			        urls: this.submitImgList,
			        current: e.currentTarget.dataset.url
			    });
			},
			//删除
			DelImg1(e) {
			    uni.showModal({
			        title: '删除',
			        content: '确定要删除这张图片？',
			        cancelText: '取消',
			        confirmText: '删除',
			        success: res => {
			            if (res.confirm) {
			                this.submitImgList.splice(e.currentTarget.dataset.index, 1)
			            }
			        }
			    })
			},
			ChooseImage2() {
				const _this = this
			    uni.chooseImage({
			        count: 9, //默认9
			        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			        sourceType: ['album', 'camera'], //从相册选择
			        success: (res) => { 
								for(let i=0;i<res.tempFilePaths.length;i++){
									const tempFilePaths = res.tempFilePaths[i]
									const uploadTask = uni.uploadFile({
										url : 'http://110.42.216.179:8000/ntda/files/uploadImages',
										filePath: tempFilePaths,
										name: 'files',
										formData: {
											'fileId': getApp().globalData.staffId
										},
										success: function (uploadFileRes) {
										 // console.log(uploadFileRes);
										 // _this.imgList = [..._this.imgList, uploadFileRes.data]
										 _this.signImgList.push(uploadFileRes.data)
										 
										}
								 });
								}
							   // console.log(_this.imgList)  
			        }
			    });
			},
			ViewImage2(e) {
			    uni.previewImage({
			        urls: this.signImgList,
			        current: e.currentTarget.dataset.url
			    });
			},
			//删除
			DelImg2(e) {
			    uni.showModal({
			        title: '删除',
			        content: '确定要删除这张图片？',
			        cancelText: '取消',
			        confirmText: '删除',
			        success: res => {
			            if (res.confirm) {
			                this.signImgList.splice(e.currentTarget.dataset.index, 1)
			            }
			        }
			    })
			},
			
			modify(subplanId){
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
								url:'/ntda/task/deleteDoingSubPlan',
								method:'POST',
								data:{
									 "subplanId": subplanId,
								}
							}).then(()=>{
								self.getData()
							}) 
							
						} else if (res.cancel) {
							// 取消
							// console.log('cancel')
						}
						
					}
				});
				
			
			}
		}
	}
</script>

<style>
	 @import "../../../colorui/main.css";
	 @import "../../../colorui/icon.css";
	/* .cu-form-group .title {
	    min-width: calc(4em + 15px);
	 } */
	 /* .grid col-4 grid-square flex-sub {
		 width: 50px;
	 } */
	 
	 
	.box{
		background-color: #FCFCFC;
		padding: 10px 10px 60px 10px;
	}
	
	.imgs{
		/* background-color: white; */
		margin: 5px ;
		border-bottom: 1px solid #CDC9C9;
		padding-bottom: 8px;
	}
	.title {
		font-size: 15px;
		margin-left: 5px;
	}
	.content {
		background-color: white;
		margin: 10px 5px 5px 5px ;
		padding: 10px;
		font-size: 15px;
		border: 0.5px solid #FF8C69;
		
	}
	span{
		font-size: 14px;
		margin-left: 15px;
		color:#545454;
	}
	.item2{
		margin-top: 5px;
		border-bottom: 1px solid #CDC9C9;
		padding-bottom: 8px;
	}
	.item3{
		margin-top: 5px;
		border-bottom: 1px solid #CDC9C9;
		padding-bottom: 8px;
	}
	
	.item3 view {
		margin: 2px;
	}
	
	.number_proportion{
		width: 80%;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	
	.icon view {
		float: right;
		color: #FF4500;
		
	}
	.icon text{
		
		font-size: 14px;
	}
	.record1 {
		color: #008B00;
	}
	.record2 {
		color: red;
	}
	.btn button {
		  /* float: bottom;
		   pad */ding-bottom: 5px;
		   background-color: #FF6347;
		   color: white;
		   text-align:center;
		  /* height: 40px;
		   width: 320px;
		   left: 30px;
		   bottom: 10px; */
		   border-radius:10px;
		   position: fixed;
		   width: 80%;
			 margin: 0 8%;
			 bottom: 10px;
	}
	
	

</style>
