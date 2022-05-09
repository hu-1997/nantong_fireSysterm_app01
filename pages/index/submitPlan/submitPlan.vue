<template>
	<view class="box">
		<view class="nav">
			<view class="imgs">任务名称   
			  <span>测试任务</span>
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
				                <!-- <view class="cu-tag bg-red"  :data-index="index">
				                    <text class='cuIcon-close'></text>
				                </view> -->
				            </view>
				            <view class="solids"  v-if="submitImgList.length<9">
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
				                <!-- <view class="cu-tag bg-red"  :data-index="index">
				                    <text class='cuIcon-close'></text>
				                </view> -->
				            </view>
				            <view class="solids"  v-if="signImgList.length<9">
				                <text class='cuIcon-cameraadd'></text>
				            </view>
				        </view>
				    </view>
				    
				</view>
			</view>
			
		</view>
		<view class="content" v-for="(item) in dataArr" >
				<view class="item1" >
					<view >
						系统名称   <span>{{item.systemName}}</span>
					</view>
					<view v-for="(item2) in item.subsystems" >
						<view class="item2">
							系统类型   <span>{{item2.subsystemName}}</span>
						</view>
						<view v-for="(item3) in item2.facilities">
							<view class="item3">
								设施名称   <span>{{item3.facilitiesName}}</span>
								<view>
										<view class="icon">
											<text class="record1" >已记录</text>
											<view >
												<uni-icons type="compose" size="18" color="#FF4500"></uni-icons>
												<text @click="jumpSubmitOptions(item3.subplanId,item3.taskId)">明细</text>
											</view>
										</view>
								</view>
							</view>
							
						</view>
					</view>
				</view>
				
				
				
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				submitImgList: [
				],
				signImgList:[],
				dataArr:[],
				taskId:''
			}
		},
		onShow(){
			this.getData()
		},
		onLoad(options){
			this.taskId = options.id
		},
		methods: {
			//获取接口数据
			async getData(){
				const res = await this.$myRequest({
					url:'/ntda/task/getDoingSubPlan?taskId=' + this.taskId
				})
				console.log(res.data)
				this.dataArr = res.data.doing
				this.submitImgList = res.data.submitImgList
				this.signImgList = res.data.signImgList
				
			},
			jumpSubmitOptions(subplanId,taskId){
				uni.navigateTo ({
					url: '/pages/index/submitPlan/submitOptions/submitOptions?id=' + subplanId + '&taskId='+ taskId
				})
			},
			// ChooseImage1() {
			// 	const _this = this
			//     uni.chooseImage({
			//         count: 9, //默认9
			//         sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			//         sourceType: ['album'], //从相册选择
			//         success: (res) => { 
			// 	           for(let i=0;i<res.tempFilePaths.length;i++){
			// 	               const tempFilePaths = res.tempFilePaths[i]
			// 				   const uploadTask = uni.uploadFile({
			// 				    url : 'http://110.42.216.179:8082/ntda/files/uploadImages',
			// 				    filePath: tempFilePaths,
			// 				    name: 'files',
			// 					formData: {
			// 					 	'fileId': getApp().globalData.staffId
			// 					},
			// 				    success: function (uploadFileRes) {
			// 				     // console.log(uploadFileRes);
			// 				     // _this.imgList = [..._this.imgList, uploadFileRes.data]
			// 				     _this.submitImgList.push(uploadFileRes.data)
							     
			// 				    }
			// 				   });
			// 				   }
			// 				   // console.log(_this.imgList)  
			//         }
			//     });
			// },
			ViewImage1(e) {
			    uni.previewImage({
			        urls: this.submitImgList,
			        current: e.currentTarget.dataset.url
			    });
			},
			//删除
			// DelImg1(e) {
			//     uni.showModal({
			//         title: '删除',
			//         content: '确定要删除这张图片？',
			//         cancelText: '取消',
			//         confirmText: '删除',
			//         success: res => {
			//             if (res.confirm) {
			//                 this.submitImgList.splice(e.currentTarget.dataset.index, 1)
			//             }
			//         }
			//     })
			// },
			// ChooseImage2() {
			// 	const _this = this
			//     uni.chooseImage({
			//         count: 9, //默认9
			//         sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			//         sourceType: ['album'], //从相册选择
			//         success: (res) => { 
			// 	           for(let i=0;i<res.tempFilePaths.length;i++){
			// 	               const tempFilePaths = res.tempFilePaths[i]
			// 				   const uploadTask = uni.uploadFile({
			// 				    url : 'http://110.42.216.179:8082/ntda/files/uploadImages',
			// 				    filePath: tempFilePaths,
			// 				    name: 'files',
			// 					formData: {
			// 					 	'fileId': getApp().globalData.staffId
			// 					},
			// 				    success: function (uploadFileRes) {
			// 				     // console.log(uploadFileRes);
			// 				     // _this.imgList = [..._this.imgList, uploadFileRes.data]
			// 				     _this.signImgList.push(uploadFileRes.data)
							     
			// 				    }
			// 				   });
			// 				   }
			// 				   // console.log(_this.imgList)  
			//         }
			//     });
			// },
			ViewImage2(e) {
			    uni.previewImage({
			        urls: this.signImgList,
			        current: e.currentTarget.dataset.url
			    });
			},
			//删除
			// DelImg2(e) {
			//     uni.showModal({
			//         title: '删除',
			//         content: '确定要删除这张图片？',
			//         cancelText: '取消',
			//         confirmText: '删除',
			//         success: res => {
			//             if (res.confirm) {
			//                 this.signImgList.splice(e.currentTarget.dataset.index, 1)
			//             }
			//         }
			//     })
			// },
		}
	}
</script>

<style>
	@import "../../../colorui/main.css";
	@import "../../../colorui/icon.css";
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
   
</style>
