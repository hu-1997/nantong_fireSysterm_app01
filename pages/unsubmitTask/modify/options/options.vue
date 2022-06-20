<template>
	<view class="box">
		<view  >
			<view class="nav">
				<view>设施名称</view>
				<text >{{this.w}}</text>
			</view>
			<view class="openSwitch">
				<view class="morecheck">
					<view>开启多选</view>
					<switch color="#FF4500" @change="switchChange" />
				</view>
				<checkbox-group @change="handleOKAll"  v-if="openSwitchVisible">
					<view>全选</view>
					<checkbox color="#FF4500" :value="isAllchecked"/>
				</checkbox-group>
				<view class="uinicons" @click="handleOK"  v-if="openSwitchVisible">
					<uni-icons type="redo-filled" size="28" color="#FF4500"></uni-icons>
				</view>
			</view>
		</view>
		<view v-for="item1 in equipmentCount" :key="item1.deviceSign">
			<view class="equipment">
				<view class="equipment_title">
						<view class="equipment_title_left">设备{{item1.deviceSign}}</view>
						<view v-if="openSwitchVisible">
							<checkbox-group @change="e => radioChange(e,item1.deviceSign)">
								<checkbox :checked="isChecked" :value="item1.deviceSign" color="#FF4500" style="transform:scale(0.8)"/>
							</checkbox-group>
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
				checkedEquipment:[],
				isAllchecked:'true',
				isChecked:false
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
				// console.log(res,4444)
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
			//多选
			radioChange(e,key){
				if(e.detail.value[0] !== undefined){
					this.checkedEquipment.push({index:parseInt(key),deviceSign:parseInt(e.detail.value[0])})
				} else if(e.detail.value[0] === undefined){
					for(let i =0;i < this.checkedEquipment.length;i++){
						if(this.checkedEquipment[i]['index'] === parseInt(key)){
							this.checkedEquipment.splice(i,1)
						}
					}
				}
			},	
			//全选
			handleOKAll(e){
				if(e.detail.value[0] === 'true'){
					this.isAllchecked = 'false'
					this.isChecked = true
					for(let i = 0;i<this.count;i++){
						this.checkedEquipment.push({index:parseInt(i+1),deviceSign:parseInt(i+1)})
					}
				} else if(e.detail.value[0] === undefined){
					this.isAllchecked = 'true'
					this.isChecked = false
					this.checkedEquipment = []
				}
			},
			//确认多选
			handleOK(){
				if(this.checkedEquipment.length > 0){
					let deviceSign = []
					this.checkedEquipment.forEach(item => {
						deviceSign.push(item['deviceSign'])
					})
					let deviceSigns = deviceSign.join(".")
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
		margin: 6px 5%;
		line-height: 22px;
	}
	.equipment_title_left{
		width: 30%;
			line-height: 22px;
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
		line-height: 21px;
	}
	.openSwitch{
		display: flex;
		flex-direction: row;
		/* justify-content: space-around; */
		margin: 4px 0;
		padding: 0 10px;
	}
	.openSwitch .morecheck{
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		width: 40%;
		line-height: 30px;
	}
	.openSwitch view{
		font-size: 18px;
		line-height: 30px;
	}
	.openSwitch switch{
		transform: scale(0.7,0.7)
	}
	.openSwitch checkbox-group{
		width: 24%;
		line-height: 30px;
		margin-left: 26%;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}
		.openSwitch .uinicons{
			height: 30px;
			/* margin-top: 2px; */
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
