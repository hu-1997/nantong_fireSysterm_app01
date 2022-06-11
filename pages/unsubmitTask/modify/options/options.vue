<template>
	<view class="box">
		<view  >
			<view class="nav">
				<view>设施名称</view>
				<text >{{this.w}}</text>
			</view>
			<view class="openSwitch">
				<view>开启多选</view>
				<switch color="#FF4500" @change="switchChange" />
				<button size="mini" @click="handleOKAll">全选</button>
				<button size="mini" @click="handleOK">确认</button>
			</view>
		</view>
		<view v-for="item1 in equipmentCount" :key="item1.deviceSign">
			<view class="equipment">
				<view class="equipment_title">
						<view class="equipment_title_left">设备{{item1.deviceSign}}</view>
						<view v-if="openSwitchVisible">
							<radio-group @change="radioChange">
								<radio :value="item1.deviceSign" color="#FF4500"/>
							</radio-group>
						</view>
				</view>
				<view v-for="(item2) in item1.facilitiesDetail" class="content" @click="jumpOptionDetail(item2.maintenanceId,item2.taskId,item1.deviceSign)" :key="item2.maintenanceId">
					<view>
						{{item2.maintenanceName}}
					</view>
					<view class="describe">
						{{item2.maintenanceDescription}}
					</view>
					<view class="weijilu" v-if="item2.record2">
						未记录
					</view>
					<view class="" v-if="item2.record1" style="color: #18BC37;">
						已记录
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
				equipmentCount:[],
				count:0,
				w:'',
				a:'',
				subplanId:'',
				taskId:'',
				arr:[],
				arr1:{ifcheck:false},
				openSwitchVisible:false,
				checkedEquipment:[]
			}
		},
		onLoad(options) {
			this.taskId = options.taskId
			this.subplanId = options.id
			this.count=options.number
			this.getInitData().then(res => {
				// console.log(res,4444)
				this.w = res[0].facilitiesName
				this.equipmentCount = res
			})
		},
		onShow(){
			this.equipmentCount = []
			this.getInitData().then(res => {
				console.log(res,4444)
				this.equipmentCount = res
			})
		},
		watch:{
			
		},
		methods: {
			//获取接口数据	
			async getData(key){
					const res = await this.$myRequest({
					url:'/ntda/task/getFacilitiesDetail?id=' + this.subplanId + '&taskId=' + this.taskId + '&deviceSign='+key
				})
				return res.data
			},
			
			//获取不同设备的信息
			async getInitData(){
				let arr = []
				for(let i  = 0;i < this.count;i++){
					arr.push(await this.getData(i+1))
				}
				return Promise.all(arr)
			},
			//开启多选
			switchChange(e){
				if(e.detail.value === true){
					this.openSwitchVisible = true
				} else{
					this.openSwitchVisible = false
					this.checkedEquipment = []
				}
			},
			//确认多选
			handleOK(){
				if(this.checkedEquipment.length > 0){
					let deviceSigns = this.checkedEquipment.join(".")
					uni.navigateTo({
						url: '/pages/unsubmitTask/modify/options/multiSelect/multiSelect?subplanId='+this.subplanId +'&taskId='+this.taskId +'&deviceSign='+deviceSigns
					})
				} else{
					uni.showToast({
							title: '请先勾选要批量填写的设备',
							icon:'error',
							duration: 2000,
					}
					)	
				}
			},
			//多选
			radioChange(e){
				this.checkedEquipment.push(parseInt(e.detail.value))
			},	
			jumpOptionDetail(maintenanceId,taskId,id){
				uni.navigateTo({
					url: '/pages/unsubmitTask/modify/options/optionDetail/optionDetail?id=' + maintenanceId + '&taskId=' + taskId+'&deviceSign='+id
				})
			}
		}
	}
</script>

<style>
	.box {
		padding: 8px;
	}
	.equipment{
		margin: 10px 0 16px 0;
		padding: 2px;
		border: 1px solid #FF4500;
		border-radius: 6px;
	}
	
	.equipment_title{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		margin: 10px 5%;
	}
	.equipment_title_left{
		width: 30%;
	}
	.nav {
		display: flex;
		margin: 5px ;
		border-bottom: 1px solid #CDC9C9;
		padding-bottom: 5px;
	}
    .nav text{
		padding-left: 13px;
		font-size: 14px;
	}
	.openSwitch{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		margin: 4px 0;
		padding: 0 10px;
	}
	.openSwitch view{
		font-size: 18px;
		line-height: 32px;
	}
	.openSwitch switch{
		transform: scale(0.7,0.7)
	}
	.openSwitch button{
		margin-right: 5%;
		color:#FFFAFA;
		background-color: #EE7942;
	}
	.content view{
		padding:5px 10px 5px 10px ;
		
	}
	.content {
		margin: 5px;
		margin-top:10px;
		border: 1px solid #DCDCDC;
		border-radius:5px;
	}
	.describe{
		color: #5B5B5B;
		font-size: 14px;
	}
	.weijilu {
		color:#EE7942;
	}
	.addEquipment{
		width: 20%;
		margin: 20px 10% 20px 70%;
	}
	.addEquipment button{
		color:#FFFAFA;
		background-color: #EE7942;
	}
	
</style>
