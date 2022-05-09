<template>
	<view >
		<view >
			<view class="nav">
				<view class="c">
					<view  >设施名称</view>
					<text >{{this.facilitiesName}}</text>
				</view>
				<view class="c">
					<view >维保内容</view>
					<text >{{this.maintenanceName}}</text>
				</view>
			</view>
			<form @submit="formSubmit">
				<view class="content">
					<view >
						<view v-if="" >
							<radio-group name="ifcheck" @change="radioChange">
								<label class="radio">
								  <radio disabled="disabled" value="true" :checked="arr1.changeValue1" /><text>能检查</text>
								</label>
								<label class="radio">
									<radio disabled="disabled" value="false" :checked="arr1.changeValue2" /><text>不能检查</text>
								</label>
							</radio-group>
						</view>
						<view class="checkContent" v-show="ifCheck" v-for="(item,index) in optionData">
							<view>
								{{item.detectionQuestion}}
							</view>
							<view v-if="item.answer1">
								<input disabled="disabled" :name="item.detectionId" :value="arr[index].inValue ? arr[index].inValue:'' " type="text" @input="(e) => inputChange(e,index)">
							</view>	
							<view>
								<radio-group :name="item.detectionId" v-for="(item2) in item.answer" disabled="disabled">
									<label class="radio" >
									  <radio disabled="disabled" value="true" :checked="arr[index].changeValue1"/><text>{{item2.answer3}}</text>
									</label>
									<label class="radio">
									  <radio disabled="disabled" value="false" :checked="arr[index].changeValue2" /><text>{{item2.answer4}}</text>
									</label>
								</radio-group>
							</view>	
							
						</view>
						<view v-show="ifCheck">
						    <form class="Box">
						       <!-- <view class="cu-bar bg-white margin-top">
						            <view class="action">
						                图片上传
						            </view>
						            <view class="action">
						                {{imgList.length}}/4
						            </view>
						        </view> -->
						        <view class="cu-form-group">
						            <view class="grid col-4 grid-square flex-sub">
						                <view class="bg-img" v-for="(item,index) in imgList" :key="index" @tap="ViewImage" :data-url="imgList[index]">
						                 <image :src="imgList[index]" value="" mode="aspectFill"></image>
						                    <!-- <view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
						                        <text class='cuIcon-close'></text>
						                    </view> -->
						                </view>
						                <view class="solids"  v-if="imgList.length<9">
						                    <text class='cuIcon-cameraadd'></text>
						                </view>
						            </view>
						        </view>
						        
						    </form>
						</view>
						<!-- 第二版上传照片结束 -->
						<view class="notCheck" v-show="notCheck">
							<view>
								不能检查的原因
							</view>
							<radio-group name="reason" @change="radioChange1">
								<label class="radio">
									<view>
										<radio disabled="disabled" value="true" :checked="arr2.changeValue1" /><text>消防设置与当前测试规范不匹配</text>
									</view>
								</label>
								<label class="radio">
									<view>
										<radio disabled="disabled" value="false" :checked="arr2.changeValue2" /><text>其他原因</text>
									</view>
								</label>
							</radio-group>
							<textarea name="detail" disabled="disabled" :value="arr2.textareaValue" placeholder="说明设施位置以及不能检查的原因"/>
							
						</view>
					</view>
					
				</view>
			</form>
			
			
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				inputValue:'',
				maintenanceName:'',
				facilitiesName:'',
				maintenanceId:'',
				ifCheck:true,
				notCheck:false,
				imgFiles:[],
				imgList: [],
				optionData:[],
				text:'',
				formdata:[],
				inputvalue:[],
				message:'',
				Disabled:false,
				arr:[],
				arr1:{},
				arr2:{},
	
			}
		},
		onLoad(options) {
			this.maintenanceId = options.id
			console.log(this.maintenanceId)
			this.taskId = options.taskId
			// this.getData()
			this.getRecordData()
		},
		methods: {
			inputChange(e,index){
				// console.log(index)
				// let obj = {}
				// obj[index] = e.detail.value
				// this.inputvalue.push(obj)  
				// console.log(this.inputvalue)
			},
			//获取接口展示数据
			async getData(){
					const res = await this.$myRequest({
					url:'/ntda/task/getMaintenanceDetail?id=' + this.maintenanceId 
				})
				this.maintenanceName = res.data.maintenanceName
				this.facilitiesName = res.data.facilitiesName
				this.optionData = res.data.maintenanceDetail
				
			},
			//获取暂存数据
			async getRecordData(){
					const res = await this.$myRequest({
					url:'/ntda/detectionRecord/getAllDetectionRecord?taskId=' + this.taskId +'&maintenanceId=' + this.maintenanceId 
				})
				this.getData()
				this.arr = res.data.arr
				this.arr1 = res.data.arr1
				this.imgList = res.data.imgList
				this.arr2 = res.data.arr2
				if (this.arr1.changeValue2 == true){
					this.ifCheck = false
					this.notCheck = true
				}
				
			},
			// formSubmit: function(e) {
			//         // console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
			//         this.formdata = e.detail.value
			// 		console.log(this.formdata)
					
			// 		// console.log(this.formdata)
			// 		// console.log(this.imgList)
			// 		// console.log(this.maintenanceId)
			// 		// console.log(this.planId)
			// 		// console.log(this.maintenanceId)
			// 		this.$myRequest({
			// 			url:'/ntda/detectionRecord/addDetectionRecord',
			// 			method:'POST',
			// 			data:{
			// 				 "formdata": this.formdata,
			// 				 "imgList": this.imgList,
			// 				 "maintenanceId":this.maintenanceId,
			// 				 "taskId":this.taskId,
			// 				 "staffId": getApp().globalData.staffId
			// 			},
			// 		}).then(()=>{
			// 			uni.showToast({
			// 			    title: '提交成功',
			// 				icon:'none',
			// 			    duration: 2000
			// 			})
			// 		})
					
			// },
			
			//......第二版上传照片
			// ChooseImage() {
				
			//     uni.chooseImage({
			//         count: 9, //默认9
			//         sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			//         sourceType: ['album'], //从相册选择
			//         success: (res) => {
			//     //         if (this.imgList.length != 0) {
			//     //             this.imgList = this.imgList.concat(res.tempFilePaths)
			// 				// // console.log(this.imgList)
			//     //         } else {
			//     //             this.imgList = res.tempFilePaths
			//     //         }
				        
			// 	        for(let i=0;i<res.tempFilePaths.length;i++){
			// 	            const tempFilePaths = res.tempFilePaths[i]
			// 	        	console.log(tempFilePaths)
			// 				const _this = this
			// 	        	const uploadTask = uni.uploadFile({
			// 	        	 url : 'http://110.42.216.179:8082/ntda/files/uploadImages',
			// 	        	 filePath: tempFilePaths,
			// 	        	 name: 'files',
			// 	        	 formData: {
			// 	        	 	'fileId': getApp().globalData.staffId
			// 	        	},
			// 	        	 success: function (uploadFileRes) {
			// 	        	  _this.imgList.push(uploadFileRes.data)
				        	  
			// 	        	 }
			// 	        	});
			// 	        }   
				           
			// 				   // console.log(_this.imgList)  
			//         }
			//     });
			// },
			ViewImage(e) {
			    uni.previewImage({
			        urls: this.imgList,
			        current: e.currentTarget.dataset.url,
					
			    });
			},
			//删除
			// DelImg(e) {
				
			//     uni.showModal({
			//         title: '删除',
			//         content: '确定要删除这张图片？',
			//         cancelText: '取消',
			//         confirmText: '删除',
			//         success: res => {
			//             if (res.confirm) {
			// 				// this.a = e.currentTarget.dataset.index
			// 				// console.log(this.imgList[a])
			//                 this.imgList.splice(e.currentTarget.dataset.index, 1)
							
			//             }
			//         }
			//     })
			// },
			//..........结束
			radioChange(e){
				if (e.detail.value == "false"){
					this.ifCheck = false
					this.notCheck = true
					
				}else{
					this.ifCheck = true
					this.notCheck = false
					
				}
			},
			radioChange1(e){
				if (e.detail.value == "true"){
					this.Disabled = true
				}else{
					this.Disabled = false
				}
			},
			
		},
	}
</script>

<style>
	@import "../../../../../colorui/main.css";
	@import "../../../../../colorui/icon.css";
	.cu-form-group .title {
	   min-width: calc(4em + 15px);
	}
	 .nav {
		 margin-top: 6px;
	 }
	  
	  .c {
		  display: flex; 
		   margin: 3px;
		   margin-left: 10px;
		  
	  }
	  .nav text {
		  margin-left:10px;
		  font-size: 13px;
		  word-wrap: break-word;
		  word-break: break-all;
		  white-space: pre-line;
	  }
	  
	  .content {
		  margin:10px;
		  border: 1px solid #FF4500;
		  border-radius:5px;
		  padding: 5px;
	  }
	  .content view{
		  padding: 3px;
		  font-size: 14px;
	  }
	  .content input{
		  background-color:#F7F7F7 ;
		  border-radius:5px;
		  padding-left: 5px;
		  height: 15px;
		  font-size: 13px;
	  }
	  .radio text{
		  padding: 5px;
		    
	  }
	  
	 .button1 button {
	 	   float: bottom;
	 	   padding-bottom: 5px;
	 	   background-color: #FF6347;
	 	   color: white;
	 	   text-align:center;
	 	   height: 40px;
	 	   width: 80px;
	 	   border-radius:15px;
	 	   position: fixed;
	 	   font-size: 15px;
	 	   line-height: 40px;
	 	   bottom: 15px;
	 	   left: 30px;
	 }
	 .button2 button {
	 	   float: bottom;
	 	   padding-bottom: 5px;
	 	   background-color: #FF6347;
	 	   color: white;
	 	   text-align:center;
	 	   height: 40px;
	 	   width: 130px;
	 	   border-radius:15px;
	 	   position: fixed;
	 	   right: 30px;
	 	   bottom: 15px;
		   font-size: 15px;
		   line-height: 40px;
	 	   
	 }
	 textarea{
		 background-color: #F7F7F7;
		 padding: 10px;
		 font-size: 15px;
	 }
	  
	  

</style>
