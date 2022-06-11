<template>
	<view class="Box" >
		<form @submit="formSubmit">
		<view class="content" >
			<view class="border_bottom">
				<view>消防系统</view>
				<view>{{taskDetailInfo.fireFightingSystem+'-'+taskDetailInfo.subFireSystem}}</view>
			</view>
			<view class="border_bottom">
				<view>消防设施</view>
				<view>{{taskDetailInfo.fireFacilities}}</view>
			</view>
				<view class="input">
					故障描述
					<input :value="inputValue" name="description"/>
				</view>
				<view class="border_bottom">
					<view>设施位置</view>
					<view>{{taskDetailInfo.location}}</view>
				</view>
				<view class="work_record_list">
					<view class="work_record">
						<text>工作记录</text>
						<textarea placeholder="请输入工作记录" name="workRecord" />
					</view>
					<view>照片记录</view>
					<view class="cu-form-group">
							<view class="grid col-4 grid-square flex-sub">
									<view class="bg-img" v-for="(item,index) in workRecodImgList" :key="index" @tap="e => ViewImage(e,'record')" :data-url="workRecodImgList[index]">
									 <image :src="workRecodImgList[index]" value="" mode="aspectFill"></image>
											<view class="cu-tag bg-red" @tap.stop="e => DelImg(e,'record')" :data-index="index">
													<text class='cuIcon-close'></text>
											</view>
									</view>
									<view class="solids" @tap="ChooseImage('record')" v-if="workRecodImgList.length<9">
											<text class='cuIcon-cameraadd'></text>
									</view>
							</view>
					</view>
				</view>
				<view class="sign_in">
					<view>现场签到照片</view>
					<view class="cu-form-group">
							<view class="grid col-4 grid-square flex-sub">
									<view class="bg-img" v-for="(item,index) in signImgList" :key="index" @tap="e => ViewImage(e,'sign')" :data-url="signImgList[index]">
									 <image :src="signImgList[index]" value="" mode="aspectFill"></image>
											<view class="cu-tag bg-red" @tap.stop="e => DelImg(e,'sign')" :data-index="index">
													<text class='cuIcon-close'></text>
											</view>
									</view>
									<view class="solids" @tap="ChooseImage('sign')" v-if="signImgList.length<9">
											<text class='cuIcon-cameraadd'></text>
									</view>
							</view>
					</view>
					</view>
					<view class="input">
						测试结果
						<radio-group @change="radioChange" class="radio" name="testResult">
								<label ><radio value="解决"  />解决</label>
								<label ><radio value="未解决" />未解决</label>
							</radio-group>
					</view>
				</view>
		<button form-type="submit" type="default" >提交</button>
			</form>
	</view>
</template>

<script>
	export default {
		onLoad(options){
			console.log(88,options)
			this.taskId = options.taskId
			this.getTasksData(this.taskId)
		},
		data() {
			return {
				taskDetailInfo:{},
				taskId:'',
				info:'非必填项',
				inputValue:'',
				signImgList:[],
				workRecodImgList:[]
			}
		},
		methods: {	
			//获取任务相关数据
			async getTasksData(params){
				const res = await this.$myRequest({
					url:'/ntda/task/getCheckOrRepariTaskDetail',
					method:'post',
					data:{taskId:params}
				})
				// console.log(9, res.data)
				this.inputValue = res.data.description
				this.taskDetailInfo = res.data
			},
			
			//获取输入框的值
			getTaskFaultDes(e){
				// console.log(e.target.value)
				this.inputValue = e.target.value
			},
			
			//获取单选框的值
			radioChange(e){
				// console.log(e.detail.value)
			},
			
			//选择图片
			ChooseImage(key) {
			    uni.chooseImage({
			        count: 9, //默认9
			        sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
			        sourceType: ['album','camera'], //从相册选择
			        success: (res) => { 
								if(key == 'record'){
									for(let i=0;i<res.tempFilePaths.length;i++){
											const tempFilePaths = res.tempFilePaths[i]
											// console.log(tempFilePaths)
											const _this = this
											const uploadTask = uni.uploadFile({
												url: 'http://110.42.216.179:8000/ntda/files/uploadImages',
												filePath: tempFilePaths,
												name: 'files',
												formData: {
													'fileId': getApp().globalData.staffId
												},
												success: function (uploadFileRes) {
													_this.workRecodImgList.push(uploadFileRes.data)		
												}
											});
										}
									} else if(key == 'sign'){
										for(let i=0;i<res.tempFilePaths.length;i++){
												const tempFilePaths = res.tempFilePaths[i]
												// console.log(tempFilePaths)
												const _this = this
												const uploadTask = uni.uploadFile({
													url: 'http://110.42.216.179:8000/ntda/files/uploadImages',
													filePath: tempFilePaths,
													name: 'files',
													formData: {
														'fileId': getApp().globalData.staffId
													},
													success: function (uploadFileRes) {
														_this.signImgList.push(uploadFileRes.data)
														
													}
												});
											}
									}
			        }
			    });
			},
			
			//删除
			DelImg(e,key) {
			    uni.showModal({
			        title: '删除',
			        content: '确定要删除这张图片？',
			        cancelText: '取消',
			        confirmText: '删除',
			        success: res => {
			            if (res.confirm && key == 'record') {
			               this.workRecodImgList.splice(e.currentTarget.dataset.index, 1)
			            } else if(res.confirm && key == 'sign'){
										 this.signImgList.splice(e.currentTarget.dataset.index, 1)
									}
			        }
			    })
			},
			
			ViewImage(e,key) {	
			    uni.previewImage({
			        urls: key == 'record' ? this.workRecodImgList : this.signImgList,
			        current: e.currentTarget.dataset.url,			
			    });
			},
			
			formSubmit: function(e) {
					// console.log(e.detail.value)
					this.$myRequest({
						url:'/ntda/task/submitCheckOrRepairTask',
						method:'POST',
						data:{
							 "taskId": this.taskId,
							 "workRecodImgList":this.workRecodImgList,
							 "signImgList": this.signImgList,
							 "description":e.detail.value.description,
							 "staffId": getApp().globalData.staffId,
							 "workRecord":e.detail.value.workRecord,
							 "testResult":e.detail.value.testResult,
							 // "tenantId": getApp().globalData.tenantId,
							 // "remark": this.remark,
							 // "deviceSign":String(this.deviceSign)
						},
					}).then((res)=>{
						console.log(res)
						// uni.showToast({
						// 		title: '提交成功',
						// 		icon:'success',
						// 		duration: 2000,
						// 		success: (res) => {
						// 			setTimeout(function(){
						// 				uni.navigateBack({})
						// 			},2000)
						// 		}
						// }
						// )	
					})
					
			},
			
			onBackPress(e) {
					uni.navigateTo({
						url: '/pages/maintenanceTask/mainTaskDetail/mainTaskDetail?taskId='+this.taskId
					});
					return true;
			},
		}
	}
</script>

<style>
	@import "../../../colorui/main.css";
	@import "../../../colorui/icon.css";
	.cu-form-group .title {
	   min-width: calc(4em + 15px);
	}
	.Box {
		/* margin:10px; */
	}
	.content{
		padding: 10px;
	}
.content .border_bottom{
		margin-top:10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.border_bottom view:nth-child(2){
		width: 80%;
		font-size: 14px;
	}
	.input{
		display: flex;
		margin-top:10px;
		border-bottom: 1px solid #EAEAEA;
		padding-bottom: 8px;
		
	}
	.taskname {
		font-size: 16px;
	}
	input {
		margin-left:15px;
		font-size: 13px;
		flex-grow:0;
		flex-shrink:0;
		width: 70%;
	}
	text {
		margin-left:15px;
		font-size: 14px;
		
	}
	.info {
		font-size: 13px;
		margin-left:15px;
		color: #8F8F8F;
	}
	.Box button {
		 /* float: bottom; */
		 /* padding-bottom: 5px; */
		 background-color: #FF6347;
		 color: white;
		 text-align:center;
	 /* height: 40px;
		 width: 320px; */
		 width: 80%;
		 border-radius:10px;
		 /* position: fixed; */
		 margin: 10px 10%;
		 /* bottom: 50px; */
	}
	.work_record_list{
		padding: 2px 0;
		border-bottom: 1px solid #EAEAEA;
	}
.work_record {
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	padding: 4px 0;
}
.work_record text{
	font-size: 16px;
	width: 18%;
	margin: 0;
}
.work_record textarea{
	width: 70%;
	height: 80px;
	margin: 4px 5%;
	border: 1px solid #FF6347;
	border-radius: 6px;
}
.radio{
	width: 50%;
	margin-left: 10%;
	display: flex;
	justify-content: space-between;
}
.sign_in{
	padding: 4px 0;
	border-bottom: 1px solid #EAEAEA;
}
.sign_in view {
	margin: 2px 0;
}
	
	

</style>
